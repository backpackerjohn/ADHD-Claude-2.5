# Scale, Lifecycle & Failure Plan — Ember

> Owner: scale & reliability. Basis: `product/product-brief.md` (source of truth), `product/llm-cost-model.md`, `contest/winner.md` (R1–R6), `contest/thesis-B.md` §8.
> Terminology per brief: Spark, Thread, Pebble, Arc, Breadcrumb, Digest, Briefing, Doorway Card, Closing Note. Never "inbox", "backlog", "overdue".

## 1. Data scale

**Per-user growth model** (heavy user ≈ P90 from cost model, ~25–30 captures/day; median far lower):

| Object | 1 month (median / P90) | 1 year (P90) | 3 years (P90) | Notes |
|---|---|---|---|---|
| Sparks | ~150 / ~900 | ~8–10k | ~25–30k | text ≤1 KB; voice transcript ≤4 KB + audio blob 100–500 KB |
| Threads | 5–10 / 20–40 | 50–120 | 100–250 | most move warm → resting → retired; count grows slowly |
| Breadcrumbs | ~30 / ~200 | ~2k | ~6k | immutable log entries, ≤1 KB |
| Digests | 1 per active Thread, overwritten | same | same | bounded: current + last N versions per Thread |
| Briefings / Doorway Cards | ~40 / ~90 | ~1k | ~3k | generated artifacts; keep last 30 hot, archive rest |

Worst-case 3-year footprint ≈ 50–100 MB text + 2–10 GB audio per heavy user. Text is trivial; **audio dominates storage cost** — original audio stays hot 90 days, then cold-tiers (user setting: keep forever / transcript-only).

**Storage architecture implications:**
- **Event log as source of truth.** Sparks, Breadcrumbs, state transitions, and corrections are append-only immutable events. Digests, Shelf views, Doorway Cards, and Arc state are **materialized projections**, rebuildable from the log (this is also the digest-corruption recovery path, §3).
- **Embeddings index** (hybrid search, M2): one vector per Spark and per Digest. 30k Sparks × small-embed (~1536-d fp16) ≈ 90 MB/user worst case — fits a per-user partition of pgvector or a managed vector store. Index sharded by user, never global. Fully re-embeddable from the event log; embedding-model version stamped per row.
- Per-user data is naturally partitionable (no cross-user reads) → row-level per-user partitioning, cheap region pinning (§5).

**Pagination / lazy-load rules:**
- Shelf: Warm row first (≤12 Threads); Resting and Retired lazy-load on scroll, 25/page.
- Thread story: Digest + last 20 events first; older events cursor-paged 50/page by time.
- Loose Sparks tray: hard cap 50 visible; older items auto-archive to a searchable state — still findable, never a growing wall.
- Search: top 20 results, cursor-paged. No unbounded queries anywhere; every list API requires cursor + limit.

**Archival tiers:** hot (warm Threads, last 90 days of events) → warm (resting Threads: in DB, out of default queries) → cold (retired Threads + audio >90 days: object storage). Digest + Closing Note stay hot so the Retired gallery renders instantly. Rehydration on Thread open is transparent, <2 s.

**Export / takeout** — always available, including free tier and post-churn (§2):
- Full JSON: every Spark, Thread, Breadcrumb, Digest, Arc, Closing Note, with timestamps and provenance.
- Markdown: one file per Thread, story-ordered, human-readable. Audio zip on request.
- Generated async; download link expires in 7 days. Export is a trust feature in a category with a dark-pattern reputation — it ships in v1.

## 2. User lifecycle

- **Day 0 (zero-corpus value).** Value must not depend on memory (the Mem/Napkin lesson, thesis-B §7).
  - First-hour value: capture relief (Catch works instantly), one Arc breakdown on the first project.
  - Graveyard import (Apple Notes/Notion, cost-capped) pre-seeds Threads so the Shelf is never empty. The Doorway renders a valid card with a single Thread.
- **Week 1 (trust formation — R3).** Misclassification is the trust killer.
  - Confidence-gated filing: below threshold → visible Loose Sparks tray ("Ember is holding these"), never a wrong guess silently filed. One-drag correction; corrections are training signal.
  - Reliability SLO is product here: filing result visible <5 s or the Spark shows "caught — filing shortly" (capture confirmation itself is local and instant regardless of backend state).
- **Month 1 (habit or lapse).** Either the deposit habit forms (D14 capture retention ≥40% target, winner.md R1) or the first lapse begins. No streaks, no red — nothing accrues shame during drift.
- **The lapse (designed-for-absence).** Notifications self-silence after ~3 ignored (M5 mandate). No guilt "we miss you" email. Breadcrumbs and Digests hold state; nightly digest maintenance stops after 7 idle days, so idle users cost ≈ $0 in LLM spend. Absence is expected, not an alarm.
- **The return (welcome-back).** The demo moment. Welcome-back Doorway: "Welcome back. Nothing is lost. Here's what's still warm." Full Briefing (≥14-day tier) on first Thread open. Return-day generation gets priority routing — never queued behind batch jobs.
- **Year 1 (memory moat).** A year of Sparks and Breadcrumbs makes Briefings qualitatively better and switching costs real. Year-in-embers recap framed as accumulation only (cumulative "ember hours", streak-free).
- **Churn.**
  - **Auto-pause billing at 45 days idle** (brief, cross-cutting): billing stops automatically; email says "we paused your billing — everything is safe."
  - Paused ≠ deleted: full data retention while paused, ≥24 months; then two warning emails + export link before any deletion.
  - Two-tap cancel always; cancel triggers an export offer.
  - **Delete-account flow:** in-app, two confirmations, immediate logical delete, hard delete from all stores and backups within 30 days (GDPR erasure), confirmation email.
- **Resurrection.**
  - One tap resumes billing next cycle (never retroactive charges) and lands on the welcome-back Doorway.
  - Long-pause Digests may be stale → background refresh on return; meanwhile the Briefing is generated from event log + last good Digest.
  - Resurrection is a first-class funnel with its own metric: return-to-capture within 7 days of reactivation.

## 3. Failure modes & recovery (per subsystem)

**Sync conflicts (offline-first capture).** Capture must never block on network. **Recommendation: last-write-wins + append-only journal, not CRDT.** Rationale:
- Sparks and Breadcrumbs are immutable events — appends never conflict, so ~95% of writes are conflict-free by construction.
- Mutable state (Thread status, Pebble accept/done, Arc edits) is low-frequency, single-user, rarely concurrent across devices → field-level LWW, with the journal preserving losing writes. A "recovered notes" view surfaces any overwritten edit; nothing is silently lost.
- CRDTs add engineering cost the one-month scope (R5) cannot afford, to solve a collaboration problem a single-user product does not have.

**ASR failure.** On-device transcription is the default; on failure or unsupported hardware → server ASR fallback; if both fail, the audio Spark stores raw, marked "voice note — transcribing later," and retries with backoff. The capture is never lost and never looks failed — worst case it is a playable voice memo held in Loose Sparks.

**LLM API outage — deterministic fallback per module:**
- M1 Catch: capture is local-first and unaffected; filing queues; Sparks sit in the Loose Sparks tray labeled "holding these for now" (the tray doubles as the outage UX).
- M2 Threads & Shelf: keyword + time search keep working (semantic degrades silently); all stored views render.
- M3 Warm Start: template Briefing assembled from stored Digest + last Breadcrumb + verbatim last 3 Sparks — "Last time: you stopped after [breadcrumb]. You said: '[quote]'." No generation required; genuinely useful.
- M4 Year Arc: current Arc and next Pebble are stored data — display unaffected; re-planning queues.
- M5 Doorway: today's card was pre-generated overnight (Batch) and already exists; fallback card = deterministic assembly (warmest Thread + stored next Pebble + newest unfiled Spark).
- Provider fallback (Haiku-class substitute) sits behind an abstraction layer. Killswitch tiers: full → Sonnet-off (Haiku everywhere) → generation-off (templates only).

**Misclassification storms** (bad model rollout / prompt regression — R3 at system scale).
- Monitor correction-rate per cohort; corrections/filings >2× baseline for 1 h → auto-revert prompt/model version and lower the filing-confidence threshold (more Sparks to the tray — visibly held beats wrongly filed).
- Every filing carries model + prompt version, so storm-window refiling is a targeted batch job; user corrections are never overwritten by refiling.

**Digest corruption / staleness.** Digests are derived state — **rebuild from the event log** (fold events in bounded windows via Haiku Batch). Checksums + schema validation on write. A Briefing reading a Digest older than the Thread's latest event triggers an async refresh and says "catching up on your latest notes." A nightly job validates Digest freshness for all warm Threads.

**Notification delivery failures.** Scheduling is deterministic (never AI), with delivery receipts where the platform allows. The Doorway never depends on the push — the card exists in-app regardless; missed morning generation → on-open generation with cached fallback. Anti-habituation copy is pre-generated in nightly Batch; if that fails, rotate from a stored copy pool. Silent failure is acceptable by design (self-silencing is a feature); duplicate sends are not — idempotency keys on every send.

**Payment failures.** 14-day grace period with full access, then free-tier feature level — **never data hostage**: capture, Shelf read, search, and export work forever regardless of billing state. Dunning is calm (two emails, no red). Involuntary churn merges into the auto-pause path, not a lockout.

**Data loss.**
- Sparks are immutable at the API layer: edits create revisions; delete is user-only and soft for 30 days.
- Event log: continuous point-in-time recovery, cross-region backups. The client keeps a local journal of unsynced captures (survives reinstall via OS backup where possible).
- Quarterly restore drills. Derived stores need no backup (rebuildable) except embeddings — re-embedding is cost-capped, so snapshot monthly.

**Reliability targets (v1 SLOs):**
- Capture local ack: <300 ms, 100% offline-capable. Filing visible: <5 s p95 (else "caught — filing shortly").
- Warm Start generation: <6 s p95, template fallback <1 s. Search: <500 ms p95 at 3-year P90 corpus.
- Durability: RPO ≤ 5 min, RTO ≤ 4 h; zero acknowledged-capture loss, ever — this is the one metric with no acceptable failure budget.

## 4. Cost at scale

**LLM spend curves.** From `llm-cost-model.md`: P90 ≈ $4.75/mo (model A) vs $3.04 (thesis-B §8 model B) — plan to the worse case, $4.75. Blended paying user ≈ $1–2/mo; free active user ≈ $0.10/mo (winner.md item 4).

| Scale (total users) | Mix (paying / free-active) | Blended LLM $/mo | All-P90 worst band |
|---|---|---|---|
| 1k | 300 / 700 | ~$0.5k | ≤ $1.7k |
| 10k | 3k / 7k | ~$5.2k | ≤ $17k |
| 100k | 30k / 70k | ~$52k | ≤ $166k |

Blended spend stays ≈6–8% of revenue at $8.99; even the implausible all-P90 band stays under the ~40% COGS line implied by the >60% gross-margin constraint. The blended-vs-P90 gap is the margin story: category retention reality (≈3.3% at 30 days, Baumel 2019) plus idle-user job shutoff (§2) means most accounts cost ≈ $0 most months.

**Infra sketch (serverless-friendly):**
- Capture path: edge function + queue — spiky, latency-tolerant after the local ack.
- Nightly Digest/Doorway pre-gen: Batch API + scheduled workers (50% token discount, per brief).
- Postgres + pgvector, per-user partitioned (managed; scales past 100k users before sharding pain). Object storage for audio and cold tier.
- No always-on GPU: ASR is on-device by default with a serverless fallback. Infra ex-LLM ≈ $0.10–0.30/user/mo at 10k+, dominated by audio storage.

**Abuse guard** (per cost model): soft cap ~2,000 AI actions/user/mo → graceful degradation to Haiku-only + queued processing; per-user spend killswitch at $5/mo → Haiku-only (brief). Additions: per-device capture rate limit (2/s sustained), import jobs cost-capped and queued (brief mandate), and signup-velocity checks so the free tier cannot be farmed as a bulk transcription/summarization API.

**Free-tier COGS cap.** Free = capture + 3 unsticks/day + 1 Thread Arc (brief). Enforced ceiling **$0.25/user/mo** (target $0.10): unsticks Haiku-only; the single Arc re-plans ≤2×/mo; no nightly Sonnet jobs; server ASR ≤10 min/mo (on-device otherwise); digest maintenance weekly, not nightly. Cap breach → queue-and-degrade, never a hard error.

## 5. Privacy & security

ADHD status is health-adjacent: treat the entire corpus as **sensitive personal data** — a Spark stream reveals diagnosis, medication, therapy, finances, sleep.

- **Data classification.** Tier 1 (content: Sparks, transcripts, Digests, Briefings, Closing Notes): encrypted, access-logged, never in analytics or server logs. Tier 2 (behavioral metadata: timestamps, counts, states): pseudonymized for product analytics. Tier 3 (account/billing). Analytics events carry counts and states, never content.
- **Encryption.** TLS 1.2+ in transit. AES-256 at rest with per-user data keys (envelope encryption): single-user erasure = key destruction, and a breach of one store yields no plaintext corpus. Audio blobs use the same per-user keys.
- **No third-party ads or trackers.** No ad SDKs, no data sale, no session-replay tooling on content surfaces. Published, short subprocessor list: LLM API (Anthropic, no-training terms), payments, email, cloud host.
- **LLM boundary.** API calls carry only the needed Digest/Sparks under zero-retention / no-training terms. User content never enters shared prompts or evals without explicit opt-in (golden-set evals come from consenting concierge users only, winner.md item 5).
- **Regional data (GDPR basics).** EU users pinned to an EU region — per-user partitioning (§1) makes this cheap. Lawful basis: contract for core features, consent for optional processing. DPAs with all subprocessors. Rights served natively: access/portability = takeout (§1); erasure = delete flow (§2, 30-day hard delete including backups). Plain-language privacy policy — this audience has documented dark-pattern trauma; honesty is positioning.
- **Support access.** Support staff see account state, billing, sync/job status, and error traces — **never Spark/Thread/Digest content**. Content access only via user-initiated, time-boxed (24 h), audited consent ("share this Thread with support"). Any admin content path is technically gated, logged, and dual-controlled.
- **Breach posture.** Incident runbook with the 72-hour GDPR notification clock; per-user encryption limits blast radius; quarterly access-log review; annual pen test before 10k users; secrets in a managed vault, no long-lived credentials in clients. Public post-mortem norm — the trust wedge (thesis-B §7) extends to how we behave on our worst day.

## 6. 10× stress questions (cross-agent scale challenge)

**M1 Catch — 10× capture rate** (250+/day: import spree, voice rambler, runaway share-sheet automation). Does zero-decision survive a firehose?
→ Local-first capture never throttles — that promise is sacred. Classification is what degrades: batched micro-triage (N Sparks per Haiku call), then queue, then tray. The abuse guard (§4) catches the pathological case; the Loose Sparks cap (§1) keeps the tray from becoming the 4,000-orphan Apple Notes wall Ember exists to replace.

**M2 Threads & Shelf — 10× corpus** (250 Threads, 100k+ Sparks after 3 years + imports). Do search and the Shelf stay fast and legible?
→ Per-user partitioned hybrid index (§1) holds p95 search <500 ms at this size. The Shelf never renders "all Threads" (Warm row + lazy tiers). The classifier auto-suggests Thread merges when it detects duplicates; archival tiers keep the hot working set small. The corpus is the moat — it must never feel heavy.

**M3 Warm Start — 10× gap and history** (returning after 6 months to a Thread with 2k events). Can a Briefing be built without a monster context?
→ Digest-first architecture is exactly this defense (R6): Briefings read the bounded Digest, never raw history, and Digests fold incrementally — cost is O(new events), not O(history). The stale-Digest refresh path (§3) covers long-idle Threads. Extreme-gap quality risk → golden-set evals include 6-month-gap fixtures.

**M4 Year Arc — 10× re-planning churn** (reality changes weekly across 20 deadline Arcs). Does re-planning spam Sonnet and destabilize plans?
→ Re-planning is debounced: batched nightly unless user-initiated or deadline-critical. Deadline math is deterministic and free. Only the next Pebble is ever shown, so plan churn is invisible by design — the UI contract absorbs the instability. Per-user Sonnet re-plan budget sits inside the $5 killswitch.

**M5 Doorway — 10× fleet at generation time** (100k users' cards due overnight; batch job late or failed). Does the morning anchor break at scale?
→ Nightly Batch pre-gen staggered by timezone with a completion deadline 2 h before local morning; a miss falls back to the deterministic card (§3), so the 90-second ritual never fails visibly. Notification fan-out uses idempotent queued sends. Capped + self-silencing notifications mean load *shrinks* with disengagement — the absence-tolerant design is also the load-shedding design.

**Standing rule.** Before build sign-off, every subsystem answers both: "what happens when the user disappears for three weeks?" (thesis-B §10) and "what happens at 10× load?" For Ember these are the same discipline — value that survives absence, and systems that survive presence.
