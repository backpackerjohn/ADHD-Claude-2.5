# Scale, Lifecycle & Failure Plan — Ember

> Owner: scale & reliability. Basis: `product/product-brief.md` (source of truth), `product/llm-cost-model.md`, `contest/winner.md` (R1–R6), `contest/thesis-B.md` §8. Terminology per brief: Spark, Thread, Pebble, Arc, Breadcrumb, Digest, Briefing, Doorway Card, Closing Note. Never "inbox", "backlog", "overdue".

## 1. Data scale

**Per-user growth model** (heavy user ≈ P90 from cost model: ~25–30 captures/day; median far lower):

| Object | 1 month (median / P90) | 1 year (P90) | 3 years (P90) | Notes |
|---|---|---|---|---|
| Sparks | ~150 / ~900 | ~8–10k | ~25–30k | text ≤1 KB; voice transcript ≤4 KB + audio blob 100–500 KB |
| Threads | 5–10 / 20–40 | 50–120 | 100–250 | most move warm → resting → retired; count grows slowly |
| Breadcrumbs | ~30 / ~200 | ~2k | ~6k | immutable log entries, tiny (≤1 KB) |
| Digests | 1 per active Thread, overwritten | same | same | bounded: 1 current + last N versions per Thread |
| Briefings/Doorway Cards | ~40 / ~90 | ~1k | ~3k | generated artifacts; keep last 30, archive rest |

Worst-case 3-year footprint ≈ 50–100 MB text + 2–10 GB audio per heavy user. Text is trivial; **audio dominates storage cost** — keep original audio 90 days hot, then cold-tier (user setting: keep forever / transcript-only).

**Storage architecture implications:**
- **Event log is the source of truth**: Sparks, Breadcrumbs, state transitions, corrections are append-only immutable events. Digests, Shelf views, Doorway Cards, Arc state are **materialized projections** rebuildable from the log (this is also the digest-corruption recovery path, §3).
- **Embeddings index** (hybrid search, M2): one vector per Spark + per Digest. 30k Sparks × small-embed (~1536-d fp16) ≈ 90 MB/user worst case — fine in a per-user partition of a pgvector/managed vector store; index sharded by user, never global. Re-embeddable from the event log (embedding model version stamped per row).
- Per-user data is naturally partitionable (no cross-user reads) → single-tenant row-level partitioning, easy region pinning (§5).

**Pagination / lazy-load rules:** Shelf loads Warm row first (≤12 Threads), Resting and Retired lazy-load on scroll, 25/page. Thread story view: Digest + last 20 events first, older events paged 50/page by time cursor. Loose Sparks tray hard-capped at 50 visible (older auto-archive to searchable state — still findable, never a growing wall). Search returns top 20, cursor-paged. No unbounded queries anywhere; every list API requires a cursor + limit.

**Archival tiers:** hot (warm Threads, last 90 days of events) → warm (resting Threads: DB, out of default queries) → cold (retired Threads + audio >90 days: object storage; Digest + Closing Note stay hot so the Retired gallery renders instantly). Rehydration on Thread open is transparent, <2 s.

**Export / takeout:** one-tap full export, always available including on free tier and post-churn (§2): (a) full JSON — every Spark, Thread, Breadcrumb, Digest, Arc, Closing Note with timestamps and provenance; (b) Markdown — one file per Thread, story-ordered, human-readable; (c) audio zip on request. Generated async, link expires in 7 days. Export is a trust feature in a category with a dark-pattern reputation — it ships in v1.

## 2. User lifecycle

- **Day 0 (zero corpus):** value must not depend on memory (Mem/Napkin lesson, thesis-B §7). First-hour value = capture relief (Catch works instantly), one Arc breakdown on the first project, graveyard import (Apple Notes/Notion, cost-capped) to pre-seed Threads. Doorway renders a valid card with one Thread.
- **Week 1 (trust formation — R3):** misclassification is the trust killer. Confidence-gated filing: below threshold → visible Loose Sparks tray ("Ember is holding these"), never a wrong guess silently filed. One-drag correction; corrections are training signal. Reliability SLO here is product: filing result visible <5 s or the Spark shows "caught — filing shortly" (capture confirmation is local and instant regardless).
- **Month 1 (habit or lapse):** either the deposit habit forms (D14 capture retention ≥40% target, winner.md R1) or the first lapse begins. No streaks, no red — nothing accrues shame during drift.
- **The lapse (designed-for-absence):** notifications self-silence after 3 ignored (M5 mandate). No "we miss you" guilt email. Breadcrumbs and Digests hold state; nightly digest maintenance stops for idle users after 7 days (cost: idle users ≈ $0 LLM). Absence is expected, not an alarm.
- **The return (welcome-back):** the demo moment. Welcome-back Doorway: "Welcome back. Nothing is lost. Here's what's still warm." Full Briefing (≥14-day tier) on first Thread open. Return-day generation gets priority routing (never queued behind batch).
- **Year 1 (memory moat):** a year of Sparks/Breadcrumbs makes Briefings qualitatively better and switching cost real. Year-in-embers recap (streak-free, cumulative "ember hours") — accumulation framing only.
- **Churn:** **auto-pause billing at 45 days idle** (brief, cross-cutting): billing stops automatically, email says "we paused your billing — everything is safe." Paused ≠ deleted: data retained in full while paused. Retention policy: paused accounts keep data ≥24 months; then two warning emails + export link before any deletion. Two-tap cancel always; cancel triggers export offer. **Delete-account flow:** in-app, two confirmations, immediate logical delete, hard delete from all stores and backups within 30 days (GDPR erasure), confirmation email.
- **Resurrection:** returning paused user → one tap resumes billing next cycle (never retroactive charges), lands on welcome-back Doorway. Digests may be stale after long pause → background refresh on return, Briefing generated from event log + last good Digest meanwhile. Resurrection is a first-class funnel with its own metric (return-to-capture within 7 days of reactivation).

## 3. Failure modes & recovery (per subsystem)

- **Sync conflicts (offline-first capture):** capture must never block on network. **Recommendation: last-write-wins + append-only journal, not CRDT.** Rationale: Sparks and Breadcrumbs are immutable events — appends never conflict, so 95% of writes are conflict-free by construction. Mutable state (Thread status, Pebble accept/done, Arc edits) is low-frequency, single-user, rarely concurrent across devices → LWW at field level with the journal preserving losers (a "recovered notes" view surfaces any overwritten edit; nothing silently lost). CRDTs add engineering cost R5's one-month scope can't afford and solve a collaboration problem Ember doesn't have (single-user product).
- **ASR failure:** on-device transcription is default; on failure or unsupported device → server ASR fallback; if both fail, the audio Spark is stored raw, marked "voice note — transcribing later", retried with backoff. The capture is never lost and never appears failed — worst case it's a playable voice memo filed to Loose Sparks.
- **LLM API outage — deterministic fallback per module:** M1 Catch: capture always local-first; filing queues, Sparks sit in Loose Sparks tray labeled "holding these for now" (the tray doubles as the outage UX). M2: keyword/time search still works (semantic degrades silently). M3 Warm Start: template Briefing from stored Digest + last Breadcrumb + verbatim last 3 Sparks — "Last time: you stopped after [breadcrumb]. You said: '[quote]'." No generation required; genuinely useful. M4: existing Arc and next Pebble are stored data — display unaffected; re-planning queues. M5: Doorway Cards are pre-generated nightly (Batch) — today's card already exists; fallback card = deterministic assembly (warmest Thread + stored next Pebble + newest unfiled Spark). Provider fallback (Haiku-class substitute) behind an abstraction layer; killswitch tiers: full → Sonnet-off (Haiku everywhere) → generation-off (templates only).
- **Misclassification storms** (bad model rollout / prompt regression, R3 at system scale): monitor correction-rate per cohort; if corrections/filings exceed 2× baseline for 1 h → auto-revert prompt/model version and drop filing-confidence threshold (more Sparks to tray — visibly held beats wrongly filed). All filings carry model+prompt version → storm-window refiling is a targeted batch job; corrections are never overwritten by refiling.
- **Digest corruption/staleness:** Digests are derived state — **rebuild from event log** (bounded: fold events in windows through Haiku Batch). Checksums + schema validation on write; a Briefing generated from a Digest older than the Thread's latest event triggers async refresh and says "catching up on your latest notes." Nightly job validates Digest freshness for warm Threads.
- **Notification delivery failures:** deterministic scheduler (never AI) with delivery receipts where platform allows; Doorway never depends on the push — the card is in-app regardless. Missed morning generation → on-open generation with cached fallback. Anti-habituation copy pre-generated in nightly Batch; if that fails, rotate from a stored copy pool. Silent failure is acceptable by design (self-silencing is a feature); duplicate sends are not — idempotency keys on every send.
- **Payment failures:** 14-day grace period with full access, then free-tier feature level — **never data hostage**: capture, Shelf read, search, and export work forever regardless of billing state. Retry dunning is calm (2 emails, no red). Involuntary churn merges into the auto-pause path, not a lockout.
- **Data loss:** Sparks are immutable at the API layer (edits create revisions; delete is user-only + soft for 30 days). Event log: continuous point-in-time recovery, RPO ≤ 5 min, cross-region backup, RTO ≤ 4 h. Client keeps a local journal of unsynced captures (survives app reinstall via OS backup where possible). Quarterly restore drills; backup of derived stores unnecessary (rebuildable) except embeddings (re-embed cost-capped, so snapshot monthly).

## 4. Cost at scale

**LLM spend curves** (from `llm-cost-model.md`: P90 ≈ $4.75/mo model-A / $3.04 model-B — plan to the worse case $4.75; blended paying user $1–2/mo; free user ≈ $0.10/mo, winner.md item 4):

| Scale (total users) | Assumed mix (paying / free-active) | Blended LLM $/mo | P90-heavy worst band |
|---|---|---|---|
| 1k | 300 / 700 | ~$0.5k (300×$1.5 + 700×$0.10) | ≤ $1.7k if all paying hit P90 |
| 10k | 3k / 7k | ~$5.2k | ≤ $17k |
| 100k | 30k / 70k | ~$52k | ≤ $166k |

Blended stays ≈6–8% of revenue at $8.99; even the (implausible) all-P90 band stays under the 40% COGS line implied by the >60% gross-margin constraint. The gap between blended and P90 is the margin story: retention reality (Baumel ≈3.3% at 30 days category-wide) means idle users cost ≈$0 because idle accounts get no nightly jobs (§2).

**Infra sketch (serverless-friendly):** capture path = edge function + queue (spiky, latency-tolerant after local ack); nightly Digest/Doorway pre-gen = Batch API + scheduled workers (50% token discount, brief mandate); Postgres + pgvector per-user-partitioned (managed, scales past 100k users before sharding pain); object storage for audio/cold tier; no always-on GPU (ASR on-device default, serverless ASR fallback). Infra ex-LLM ≈ $0.10–0.30/user/mo at 10k+, dominated by audio storage.

**Abuse guard** (per cost model): soft cap ~2,000 AI actions/user/mo → graceful degradation to Haiku-only + queued processing; per-user spend killswitch at $5/mo → Haiku-only (brief). Add: per-device rate limit on capture API (2/s sustained), import jobs cost-capped and queued (brief: "cost-capped"), signup velocity checks so free tier can't be farmed as a bulk transcription API.

**Free-tier COGS cap:** free = capture + 3 unsticks/day + 1 Thread Arc (brief). Enforced ceiling **$0.25/user/mo** (target $0.10): unsticks are Haiku-only, single Arc re-plans max 2×/mo, no nightly Sonnet jobs, no server ASR beyond 10 min/mo (on-device otherwise), digest maintenance weekly not nightly. Cap breach → queue-and-degrade, never hard error.

## 5. Privacy & security

ADHD status is health-adjacent: treat the entire corpus as **sensitive personal data** (a Spark stream reveals diagnosis, meds, therapy, finances, sleep).
- **Classification:** tier 1 (content: Sparks/transcripts/Digests/Briefings) — encrypted, access-logged, never in analytics or logs; tier 2 (behavioral metadata: timestamps, counts) — pseudonymized for product analytics; tier 3 (account/billing). Analytics events carry counts and states, never content.
- **Encryption:** TLS 1.2+ in transit; AES-256 at rest with per-user data keys (envelope encryption) so single-user erasure = key destruction and a breach of one store ≠ plaintext corpus. Audio blobs encrypted with the same per-user keys.
- **No third-party ads or trackers.** No ad SDKs, no data sale, no session-replay tools on content surfaces. Subprocessor list is short and published: LLM API (Anthropic; no-training terms), payments, email, cloud host.
- **LLM boundary:** API calls carry only the needed Digest/Sparks, under zero-retention/no-training terms; user content never enters shared prompts or evals without explicit opt-in (golden-set evals from consenting concierge users only, per winner.md item 5).
- **Regional data (GDPR basics):** EU users pinned to EU region (per-user partitioning makes this cheap, §1); lawful basis = contract for core, consent for optional processing; DPAs with all subprocessors; rights supported natively — access/portability = takeout (§1), erasure = delete flow (§2, 30-day hard delete incl. backups); privacy policy in plain language (audience with documented dark-pattern trauma; honesty is positioning).
- **Support access:** support staff see account state, billing, sync/job status, error traces — **never Spark/Thread/Digest content**. Content access only via user-initiated, time-boxed (24 h), audited consent grant ("share this Thread with support"). Admin content access technically gated, logged, and dual-controlled.
- **Breach posture:** incident runbook with 72-h GDPR notification clock; per-user encryption limits blast radius; quarterly access-log review; annual pen test before 10k users; secrets in managed vault, no long-lived credentials in clients. Public post-mortem norm — the trust wedge (thesis-B §7) extends to how we behave on our worst day.

## 6. 10× stress questions (cross-agent scale challenge)

- **M1 Catch — 10× capture rate (250+/day: import spree, voice rambler, or runaway share-sheet automation).** Does zero-decision survive a firehose? *Mitigation:* local-first capture never throttles (the promise is sacred); classification is what degrades — batched micro-triage (N Sparks per Haiku call), then queue, then tray. Abuse guard (§4) catches the pathological case. Loose Sparks cap (§1) keeps the tray from becoming the 4,000-orphan Apple Notes wall we exist to replace.
- **M2 Threads & Shelf — 10× corpus (250 Threads, 100k+ Sparks after 3 years + big imports).** Does search stay fast and the Shelf legible? *Mitigation:* per-user partitioned hybrid index (§1) keeps p95 search <500 ms at this size; Shelf never renders "all Threads" (Warm row + lazy tiers); auto-suggest Thread merges when the classifier sees duplicates; archival tiers keep hot working set small. The moat is the corpus — it must never feel heavy.
- **M3 Warm Start — 10× gap and 10× history (returning after 6 months to a Thread with 2k events).** Can a Briefing be built without a monster context? *Mitigation:* digest-first architecture is exactly this defense (R6): Briefings read the bounded Digest, never raw history; Digests fold incrementally so cost is O(new events), not O(history). Stale-Digest refresh path (§3) covers long-idle Threads. Quality risk at extreme gaps → golden-set evals include 6-month-gap fixtures.
- **M4 Year Arc — 10× re-planning churn (reality changes weekly across 20 deadline Arcs).** Does re-planning spam Sonnet and destabilize plans? *Mitigation:* re-planning is debounced (batched nightly unless user-initiated or deadline-critical); deadline math is deterministic and free; only the next Pebble is ever shown, so plan churn is invisible by design — the UI contract absorbs the instability. Per-user Sonnet re-plan budget inside the $5 killswitch.
- **M5 Doorway — 10× fleet at generation time (100k users' cards due "overnight"; batch job late or failed).** Does the morning anchor break at scale? *Mitigation:* nightly Batch pre-gen staggered by timezone with a completion deadline 2 h before local morning; miss → deterministic fallback card (§3), so the 90-second ritual never fails visibly. Notification fan-out through idempotent queued sends; capped + self-silencing means load *shrinks* with disengagement — the absence-tolerant design is also the load-shedding design.

**Standing rule:** every subsystem must answer "what happens when the user disappears for three weeks?" (thesis-B §10) *and* "what happens at 10× load?" before build sign-off. For Ember these are the same discipline: value that survives absence, and systems that survive presence.
