# Ember — AI Brain & Behavior Specification

> Conforms to `product/product-brief.md` (source of truth) and `product/llm-cost-model.md` (authoritative pricing + routing). Risk owners: R2 (delayed payoff), R3 (misclassification vs zero-decision trust), R6 (briefing quality is the product) per `contest/winner.md`. Pricing basis: Haiku 4.5 $1/$5 per MTok, Sonnet $3/$15; cache reads 0.1×; Batch −50%.

## 1. Model routing table

Every LLM call in the product. Defaults per cost-model rule 1: **Haiku-first; Sonnet only on complexity triggers**. All calls share a frozen system prompt + slowly-changing user-profile block behind a cache breakpoint (≈2,000–3,000 tok, billed at 0.1× on reads); volatile content (digest, recent Sparks) sits after it.

| Module | Call | Tier | Mode | In tok (uncached + cached@0.1× = effective) | Out tok | Cache strategy | Cost/call |
|---|---|---|---|---|---|---:|---:|
| M1 | Capture filing/triage (per Spark) | Haiku | Sync (<2s target) | 300 + 2,000 = 500 eff | 120 | Frozen sys prompt + thread-name index in cached prefix | $0.0011 |
| M1 | Voice transcript cleanup (server fallback only; on-device default) | Haiku | Sync | 700 eff | 300 | Same prefix | $0.0022 |
| M1 | Graveyard import triage (one-time) | Haiku | Batch | 800 eff/item | 150 | Shared prefix across items | $0.0008/item, **hard cap $1.50 per ImportBatch (per-batch)** |
| M2 | Nightly digest maintenance (only threads touched that day) | Haiku | Batch | 4,000 eff | 400 | Prefix cached; digest-delta prompt | $0.0030 |
| M2 | Search embedding (per Spark + query) | Embed model | Async on capture | ~200 | — | n/a (vectors stored) | ~$0.00003 |
| M2 | Search rerank / NL recall answer | Haiku | Sync | 2,000 eff | 250 | Prefix cached | $0.0033 |
| M2 | Auto-Breadcrumb prose polish (optional evening pass — Breadcrumbs themselves are written deterministically at exit from exit telemetry: last screen/action/edited item + any capture text) | Haiku | Batch | 1,500 eff | 300 | Prefix cached | $0.0015 |
| M3 | Warm Start — brief/full (≥3 days) | Sonnet | Sync | 2,800 eff | 350 | Prefix cached; digest after breakpoint | $0.0137 |
| M3 | Unstick exchange (max 3/session) | Haiku | Sync | 800 eff | 150 | Prefix cached | $0.0016 |
| M3 | Closing note (Retire with honor) | Sonnet | Sync | 1,500 eff | 250 | Prefix cached | $0.0083 |
| M4 | Arc decomposition (new arc / "go deeper") | Sonnet | Sync | 1,800 eff | 700 | Prefix cached | $0.0159 |
| M4 | Standard breakdown / next-Pebble regen | Haiku | Sync | 1,000 eff | 400 | Prefix cached | $0.0030 |
| M4 | Context-aware re-planning (reality changed) | Sonnet | Batch (overnight) | 2,000 eff | 500 | Prefix cached | $0.0068 |
| M5 | Doorway card pre-generation | Sonnet | Batch (nightly) | 3,000 eff | 500 | Prefix cached | $0.0083 |
| M5 | Welcome-back Doorway (return after ≥7d absence) | Sonnet | Sync | 3,000 eff | 400 | Prefix cached | $0.0150 |
| M5 | Notification copy pool (anti-habituation, ~1×/week) | Haiku | Batch | 600 eff | 400 | Prefix cached | $0.0013 |

*The whisper tier (<3 days away) has NO row here: it is deterministic — a template of the last Breadcrumb + the Pebble chip, no AI call — saving ≈$0.0013 × ~30 whispers/mo ≈ $0.04/mo at high usage. Brief/full tiers are unchanged (Sonnet).*

Escalation triggers to Sonnet (cost-model rule 1): re-entry gap ≥ 3 days (aligned to the brief's briefing tiers; the cost model's "≥7 days" wording is superseded — see build-log), arc complexity (≥ N milestones or deadline-bearing), explicit user tap "go deeper".

**Monthly roll-up at the cost model's high-usage profile** (30 filings/day, 2 briefings/day, 3 unsticks/day, 1 Doorway/day, ~15 arc ops/mo, nightly digests, 3 searches/day):

| Bucket | Monthly |
|---|---:|
| M1 filing + transcript cleanup (900 + 150 calls) | ≈ $1.32 |
| M2 digests + breadcrumbs + search + embeddings | ≈ $0.48 |
| M3 briefings (60, brief/full — whispers are deterministic, $0) + unstick (90) + closing notes | ≈ $0.90 |
| M4 decomposition + standard breakdown + re-planning | ≈ $0.90 |
| M5 Doorway pre-gen + welcome-back + copy pool | ≈ $0.31 |
| Subtotal | ≈ $3.91 |
| + 25% safety margin (retries, longer contexts) | ≈ $0.98 |
| **Total, high-usage** | **≈ $4.89 — under the $5.99 ceiling; median user ≈ $1–2** |

Rounding differs slightly from the cost model's $4.75 because this table adds closing notes, welcome-back Doorways, and the copy pool; both land under guardrail. Any new AI call must be added to this table before it ships.

## 2. Prompt contracts (the 5 calls that matter)

Common rules for all five: **inputs are digest-first — the model never receives raw Thread history**, only the maintained Digest + a bounded window of recent Sparks/Breadcrumbs (each with `spark_id`/`crumb_id`). Every request uses the shared envelope:

```json
{
  "contract": "briefing.v1",          // versioned; part of the cache key
  "profile": { "...": "cached prefix: frozen system prompt + user profile block" },
  "context": { "digest": "...", "sparks": [{"spark_id": "spk_...", "text": "verbatim"}],
                "breadcrumbs": [{"crumb_id": "brc_...", "text": "..."}] },
  "signals": { "days_away": 0, "capacity": "full|medium|low", "exchange_count": 0 }
}
```

Output is structured JSON validated against schema; invalid JSON → one retry → deterministic fallback (§5). Tone: warm, second person, zero guilt. **Banned words (hard post-filter, regenerate on hit): overdue, streak, failed, behind, lazy** (plus brief-banned nouns: task list, to-do, backlog, inbox). Grounding: any text presented as the user's own words must be a **verbatim substring of a supplied Spark, citing its `spark_id`**; a post-generation checker verifies substring match and drops/regenerates on failure. If supplied memory is insufficient, the model must return the honest-fallback field, never invent.

### 2.1 Filing (M1)
- **In:** Spark text (≤500 tok), thread index (id, name, 1-line digest headline, last-touched), user's last 10 corrections as few-shot exemplars (§6).
- **Out (merged filing schema — canonical, referenced by M1 §8):** `{decision: "file"|"propose_new_thread"|"loose", thread_id?, alt_thread_ids: [≤2], reason: ≤12 words, confidence}`
- **Tone:** reason string is user-visible on tap ("sounded like the Etsy shop"); warm, never defensive.
- **Grounding:** may only reference thread_ids present in the index. No confidence inflation: uncertainty → `loose`.
- **Cap:** output ≤ 120 tok.

### 2.2 Warm Start briefing (M3)
- **In:** Thread Digest, last 3 Breadcrumbs, 3–8 candidate Sparks (verbatim, with ids), days_away, capacity signal, candidate Pebbles.
- **Out:** `{tier: "whisper"|"brief"|"full", where_you_were: str, your_own_words: [{spark_id, quote}], why_you_cared: str, whats_changed?: str, tiny_step: {pebble_id, text, est_minutes ≤ 60}, insufficient_memory: bool}`
- **Tone:** "Welcome back. Nothing is lost." energy; second person; quotes framed as the user's past self talking to them.
- **Grounding:** every `quote` verbatim-substring-verified against its `spark_id`; `whats_changed` only from Digest facts. If `insufficient_memory`, UI renders honest fallback: "You captured only a little here — here's what I have" + raw Sparks list.
- **Caps:** whisper ≤ 40 words; brief ≤ 120 words; full ≤ 250 words; exactly one tiny_step.

### 2.3 Unstick (M3)
- **In:** current Pebble, Thread Digest headline, exchange transcript so far (this session only), exchange_count.
- **Out:** `{stage: "name_feeling"|"shrink_stakes"|"physical_first_move"|"suggest_break", message: str, shrunk_step?: {text, est_minutes ≤ 2}}`
- **Tone:** validating, never diagnostic; names feelings as options, never asserts them ("Is it that the stakes feel big?").
- **Grounding:** references only the supplied Pebble/Digest; no claims about the user's history beyond inputs.
- **Caps:** message ≤ 60 words; **hard cap 3 exchanges, then `stage: suggest_break`** (enforced in code, not by the model).

### 2.4 Arc decomposition (M4)
- **In:** Thread Digest, user's stated goal Spark(s) (verbatim + ids), deadline?, already-done facts from Digest, stated energy patterns from profile block.
- **Out:** `{arc_id, milestones: [{title, target_window, rationale ≤ 20 words}] (3-8), first_pebble: {text, est_minutes ≤ 60}, backwards_plan?: {deadline, lead_time_days}, assumptions: [str]}`
- **Tone:** milestones phrased as arrivals ("Draft exists"), never obligations.
- **Grounding:** must not invent completed work; anything uncertain goes in `assumptions` (surfaced for one-tap confirm). Only `first_pebble` is ever shown — the milestone list stays behind a disclosure (never the wall of steps).
- **Caps:** ≤ 8 milestones; exactly one first_pebble; total output ≤ 700 tok.

### 2.5 Doorway (M5)
- **In:** ≤3 warm-thread Digest headlines, one deadline-arc status, 1 candidate resurfaced Spark (verbatim + id), capacity dial value, yesterday's Doorway (for novelty), notification-copy pool.
- **Out:** `{greeting: str, items: [{type: "warm_thread"|"pebble"|"resurfaced_spark", thread_id, text, spark_id?}] (≤3), suggested_pebble: {pebble_id, text, est_minutes}, capacity_variant: "full"|"low"}`
- **Tone:** a friend catching you up, not a manager assigning; low-capacity day reads as a **complete** plan ("Today: just the 1099 email. That's a whole day."), never a degraded one.
- **Grounding:** resurfaced Spark verbatim + id-cited; no item may reference a Thread absent from inputs; greeting must differ from yesterday's (checked in code).
- **Caps:** whole card readable in ≤ 90 seconds ≈ ≤ 130 words; ≤ 3 items; exactly 1 suggested Pebble; 1 resurfaced Spark max.

## 3. Behavior rules

**Confidence gating (R3).**

| Filing confidence | Action | UI |
|---|---|---|
| ≥ 0.80 | Auto-file | "Caught → filed to *Etsy shop*" toast, one-tap undo |
| 0.55–0.79 | File provisionally | Marked "best guess" on the Thread; one-drag correction |
| < 0.55 | **Loose Sparks tray** | "Ember is holding these" — visible, never a hidden inbox, never silently guessed |

New-thread creation always requires ≥ 0.80 or lands Loose (a wrong new thread is costlier to trust than a held Spark). First 7 days per user: all thresholds +0.05 — zero-decision trust is won in week one or never. Every correction feeds the exemplar store (§6).

**When AI must NOT act:**
- Never writes to the calendar; never auto-schedules anything (calendar is read-only, phase 2).
- Never initiates contact beyond the capped, self-silencing notification budget:

  | Channel | Cap | Self-silence rule |
  |---|---|---|
  | Doorway morning ping | 1/day | Ignored 3 consecutive days → drops to 2/week; ignored again → silent until next open |
  | Deadline-arc lead ("leave-by" class, phase 2) | ≤ 2/week per arc | Only from deterministic backwards-planned math; copy from pre-generated pool |
  | Welcome-back | 1 per absence ≥ 14 days | Never repeats for the same absence |

  Copy is drawn from the anti-habituation pool (M5); scheduling is deterministic code (§4) — the model writes words, never chooses moments.
- Never makes medical, diagnostic, or therapeutic claims; never role-plays coach/therapist; unstick is a bounded script, **hard-capped at 3 exchanges then suggests a break** — enforced in application code.
- Never deletes, merges, or retires a Thread on its own; Retire with honor is user-initiated only.
- Never fabricates memory: no quote without a verbatim-verified spark_id, ever.

**Safety posture (deterministic, not model judgment).** A local pattern + Haiku-flag pipeline screens captures for crisis language (self-harm, harm to others, acute distress). On trigger: the Spark files privately with no AI commentary, and the UI shows a static, human-written resource card (988 Suicide & Crisis Lifeline, Crisis Text Line, intl. equivalents) — **never AI-improvised counseling**. The rule is deterministic: flag → card, no model discretion in the response path. False-positive tolerance is deliberately high; the card is warm and dismissible.

## 4. Deliberately NOT AI (from brief §AI architecture, thesis-B §6)

| Function | Why deterministic |
|---|---|
| Reminders, notification scheduling, deadline math | Must be inspectable and trustworthy; no model deciding when to interrupt |
| Calendar auto-scheduling | Doesn't exist at all — Motion's failure mode; Ember never packs a day |
| Micro-rewards (<300ms) + novelty rotation | Delay-discounting: reward cannot wait on an API round-trip (R2) |
| Progress ("ember hours"), amnesty mechanics | Plain accumulation math; guilt-free by construction, not by prompt |
| Billing, trial, auto-pause | Honesty features must be code, not model behavior |
| Capture recording + default transcription | Must work offline, instantly; on-device transcription default; AI applies after the fact |
| Search indexing (keyword + vector store) | Deterministic infrastructure; only rerank/recall-answer uses a model |
| Chat as primary surface | Doesn't exist; blank-page decisions are the disease, buttons and cards are the cure |

## 5. Degradation ladder

**LLM API down (or p95 latency > 6s) — per-module deterministic fallbacks:**

| Module | Fallback behavior |
|---|---|
| M1 Catch | Capture always succeeds locally → filing queued; Sparks land in Loose Sparks with "Ember will file these when it's back." Capture is never blocked by AI. |
| M2 Threads | Last good Digest serves read paths; search degrades to keyword + time filters (vector store still queries; rerank skipped). Digest maintenance queues. |
| M3 Warm Start | Deterministic briefing template: last Breadcrumb verbatim + last 3 Sparks verbatim (already grounded by construction) + last accepted Pebble + the four buttons. Unstick falls back to the static 4-step script (name the feeling → shrink the stakes → 2-minute physical move → break), fully pre-written, no generation. Closing note falls back to a template the user can edit. |
| M4 Year Arc | Existing Arc + stored next Pebble render unchanged; decomposition and re-planning queue with honest copy ("I'll have a plan for this tonight"). Deadline math is deterministic and unaffected. |
| M5 Doorway | Renders from last pre-generated card refreshed by deterministic deadline math; notification copy pulls from the stored pool (pool depth ≥ 14 days). Welcome-back falls back to the M3 template. |

Nothing user-facing ever blocks on a model call except sync briefings/unstick, which time out to fallbacks at 6s. Queued work drains through Batch on recovery, oldest first.

**Spend killswitch:** per-user metered spend trending **> $5/mo → Haiku-only + Batch-only** (Sonnet calls rerouted to Haiku with the same contracts; all sync-optional calls queued to nightly Batch). Soft cap ~2,000 AI actions/mo → graceful queueing (cost-model abuse guard). Silent to the user except slightly plainer briefings.

**Offline:** capture, night mode, viewing Threads/Digests/last Doorway all work from local store; every AI action queues and reconciles on reconnect. Import pauses offline.

## 6. Learning loop

**Misfile corrections as training signal (no fine-tuning).** Every one-drag correction writes an exemplar `{spark_text, wrong_thread, right_thread, timestamp}` to a **per-user few-shot exemplar store**; the filing prompt injects the 10 most relevant exemplars (embedding-similarity to the incoming Spark, recency-weighted). Store is per-user, capped at 200 (LRU by retrieval usefulness), user-viewable and deletable. Exemplars sit after the cache breakpoint so the cached prefix stays stable. Expected effect: per-user filing accuracy climbs within week one — the R3 mitigation. No gradient ever leaves the user's account; "learning" is retrieval, not weights.

**Briefing feedback.** Every briefing carries a quiet "that's not what I meant" affordance → user picks what was off (wrong emphasis / wrong quote / wrong step / tone) → (a) the Digest gets a correction annotation Haiku applies at next maintenance, (b) the thumbs-down + category logs to the eval set (§7). Two strikes on one Thread → next briefing runs with the honest-fallback bias (quote more, synthesize less).

**Privacy stance:** user content **never trains shared or foundation models**; exemplar stores and eval-set entries are per-user unless explicitly donated. Digests are user-visible and deletable (deleting a Digest triggers regeneration from retained Sparks, or full forget). One-tap export of everything (Sparks, Threads, Digests, Breadcrumbs) in open formats. Concierge/Wizard-of-Oz transcripts used for golden sets are consented and anonymized.

## 7. Eval plan

- **Golden sets:** seeded from Wizard-of-Oz concierge transcripts (winner.md steal-list #5): real capture streams + operator-written filings, Doorways, and briefings become reference outputs. Minimum viable set before launch: 200 filing cases (incl. ambiguous/multi-thread Sparks), 40 briefing scenarios spanning whisper/brief/full and thin-memory cases, 20 arc decompositions, 15 Doorway days, 12 unstick sessions. Grown weekly from production thumbs-downs (anonymized, consented).
- **Filing accuracy:** target **≥ 85% correct-thread at v1** (auto-file decisions, measured against user corrections as ground truth), **measured weekly**; Loose-Spark rate tracked as companion metric (goal: <20% after week one per user). Below 80% for two weeks → routing/prompt review is mandatory.
- **Briefing quality rubric (R6)** — each golden-set briefing scored 1–5 on: **Groundedness** (every claim traceable to Digest/Spark; any fabricated quote = automatic 1), **Warmth** (tone rules, zero guilt, banned-word absence), **Actionability** ("could the user restart from this alone?" — the concierge threshold ≥ 50% yes, target 70%). Scored by Opus-as-judge (offline, per cost-model role) with a 10% human-audited sample.
- **Regression gates:** no prompt, model, or threshold change ships without a golden-set run; gate = no metric drops > 2 points absolute vs current baseline, zero banned-word emissions, zero grounding failures. Unstick and safety-card paths have dedicated red-team suites (crisis phrasing variants must always yield the static card).
- **Live monitors:** weekly dashboards for filing accuracy, briefing thumbs-down rate by category, briefing→action-within-48h rate (the concierge metric), per-user spend distribution vs killswitch line.
- **Cadence & ownership:** golden-set runs on every prompt PR (CI-gated); accuracy/rubric dashboards reviewed weekly; a monthly deep-dive re-reads a sample of raw briefings against Digests to catch drift the judge misses. The AI-systems owner signs off on every gate.

## Change control

This spec conforms to the brief; changes to routing tiers, thresholds, banned words, safety behavior, or any new AI call require editing this file, passing the §7 regression gates, and a build-log entry. The five prompt contracts in §2 are versioned; a contract version bump invalidates cached prefixes deliberately (cache key includes contract version).
