# Module: M5 — Doorway

> Conforms to `product/modules/_TEMPLATE.md`. Terminology and object model per `product/product-brief.md` (source of truth). Binding synthesis items absorbed here: capacity dial (from Anchor), nightly Batch pre-generation of anti-habituation notification copy (from Anchor), bounded 90-second morning card (Ember core #14).

## 1. Purpose

The Doorway is Ember's daily anchor: one bounded morning card (≤3 items, done in ≤90 seconds) that replaces the anxious mental scan of "what am I forgetting?" — the single question ADHD working memory cannot answer ([ATTN Center](https://attncenter.nyc/understanding-working-memory-in-adhd-how-to-remember-not-to-forget/)). It is deliberately small so it can never become "another thing to fail at," Sunsama's documented failure mode ([rivva](https://blog.rivva.app/p/best-sunsama-alternatives-for-adhders)). It carries the capacity dial (a low-capacity day renders a *complete* plan, never a degraded one), the anti-habituation notification engine (ADHD brains tune out repeated identical alarms — [My Patient Advice](https://mypatientadvice.co.uk/knowledge-base/why-do-adhd-brains-still-ignore-phone-alarms/), [Sprout](https://www.sproutapp.tech/blog/adhd-reminder-app); novelty re-engages attention — [Brain](https://academic.oup.com/brain/article/141/5/1545/4934119)), the landing surface for night-captured Sparks (insomnia affects 66.8% of ADHD adults — [Brevik et al.](https://pubmed.ncbi.nlm.nih.gov/28547881/)), and the welcome-back Doorway that turns absence — the category norm (≈3.3% 30-day retention, [Baumel 2019](https://www.jmir.org/2019/9/e14567/)) — into a loyalty event instead of a churn event. No streaks, no overdue, no red ([Kabit](https://kabitapp.com/blog/habit-tracker-adhd), [medRxiv streak-backfire](https://www.medrxiv.org/content/10.1101/2024.12.26.24319676.full.pdf)).

## 2. User goals

- I want one small glance that tells me what's warm today, so that I don't run the anxious "what am I forgetting?" scan myself.
- I want the app to fit my actual capacity today, so that a bad brain day gets a real, complete plan instead of a shamed-down version of a good day's plan.
- I want notifications that respect me — few, varied, and self-quieting — so that I don't tune the app out like every alarm before it.
- I want my 1am thoughts to be waiting for me in the morning, so that capturing at night costs me nothing and wakes no one.
- I want coming back after weeks away to feel like a welcome, not an audit, so that returning is easier than avoiding.
- I want to be able to skip today entirely with zero consequence, so that missing a day never becomes a reason to quit.

## 3. Objects

- **Doorway Card** (owned): today's bounded surface. Fields owned here: `card_date`, `card_type` (today's · welcome-back), `dial_position` (full · medium · low_spoon), `items[]` (max 3: warm-threads line, suggested Pebble ref, resurfaced Spark ref + action), `generated_at`, `generation_source` (batch · on_demand · fallback), `skipped_at`, `closed_at`.
- **Capacity Dial state** (owned): `current_position`, `last_changed_at`, per-day history (for pre-gen defaults only — never surfaced as a judgment or trend).
- **Notification schedule** (owned, deterministic): `slots[]` (≤2/day), `quiet_hours` (default 22:00–08:00, user-editable), `copy_pool_refs`, `ignore_count`, `silenced` flag.
- **Touched, not owned:** Thread (reads warm/resting states — M2), Pebble (surfaces ONE suggested — M4/M3), Spark (surfaces ONE resurfaced, always with an attached action; routes night captures — M1), Digest (read as generation context), Briefing (welcome-back card links into Warm Start — M3), billing-pause state (cross-cutting honest billing).

## 4. Lifecycle

**Doorway Card**
- `pre_generated` (nightly Batch) → `ready` (at user's local wake window) → `viewed` → `closed` (acted or dismissed) or `skipped` (never viewed by end of day).
- Dial change while `ready`/`viewed` → `regenerating` → `ready` (a new complete card, not a filtered one).
- At local midnight today's card is archived and tomorrow's silently replaces it — there is no state called "missed" and nothing rolls into "overdue."
- Retention: skipped cards kept 30 days (debug only) then deleted; closed cards kept 12 months to feed variety checks, then deleted.
- One card per calendar day maximum. When absence ≥7 days, the welcome-back card supersedes today's card.

**Capacity Dial**
- Persistent setting; defaults to `full`. Created at onboarding, never expires.
- Remembered day-to-day: a low-spoon week stays low-spoon without re-asking.
- Changing it regenerates today's card and sets the pre-gen default for tomorrow's Batch run.
- Never auto-resets, never trends, never commented on — dial history exists only as a pre-gen input.

**Notification schedule**
- Created at onboarding (user picks a morning window); lives until edited; deterministic forever.
- `ignore_count` increments per notification neither opened nor dismissed-with-intent; at N=5 consecutive ignores → `silenced`, and one gentle check-in is queued in its place ("Want us to keep knocking, or just leave the card by the door?").
- Any Doorway open resets `ignore_count` to 0. `silenced` clears only via the check-in or settings — never automatically.

## 5. Actions

- **Open Doorway** — app launch to Doorway tab / tap notification. Input: none. Effect: shows today's card; resets notification ignore count. Undo: n/a.
- **Do the Pebble** — button on the card's Pebble item. Effect: opens the Thread with its Warm Start whisper/brief (M3); Pebble → accepted. Undoable (Pebble returns to suggested).
- **Look at resurfaced Spark** — tap the Spark item. Effect: opens Spark in its Thread with its one attached action (add to Thread note / turn into Pebble / archive). All choices undoable.
- **Set capacity dial** — 3-position control at top of card (full · medium · low-spoon). Effect: regenerates today's card at the new capacity; position persists. Undoable by re-setting the dial.
- **Not today (skip)** — "Not today" text button, always visible. Effect: closes the card; nothing else happens, ever. No undo needed; card remains reachable until midnight.
- **Snooze card item** — swipe on any item. Effect: item is excluded from today's and tomorrow's generation. Undo via toast.
- **Edit quiet hours / notification window / re-enable notifications** — settings, and inline from the gentle check-in. Deterministic effect, immediate.
- **Welcome-back "show me"** — single button on the welcome-back card. Effect: opens the Shelf's Warm row (M2) or the top Thread's full Briefing (M3).

## 6. States

- **Normal day (ideal):** card with ≤3 items —
  1. warm-threads line ("Three threads are warm today"),
  2. ONE suggested Pebble with size estimate ("find the 1099 email — about 5 min"),
  3. ONE resurfaced Spark with an action attached ("Tuesday's logo idea — want to look?").
  Readable in ≤90 seconds; no fourth item exists under any condition.
- **Low-spoon day:** dial at low-spoon renders a COMPLETE plan of exactly one tiny thing, framed as the whole plan ("Today: one 2-minute thing, and that's a full day"). Never a visibly trimmed list, never a "reduced mode" label, no residue of the fuller card. Medium: 2 items with a smaller Pebble.
- **Welcome-back (≥7 quiet days):** headline "Welcome back. Nothing is lost. Here's what's still warm." At most 2 warm Threads and one tiny step. Explicitly NO backlog wall, no count of days away, no accumulated list. If auto-pause fired (45 idle days), one added line: "We paused your billing while you were away — everything is safe."
- **Nothing-warm / empty (fresh account, or every Thread resting):** no fake card. A single capture prompt instead ("Nothing's warm yet. Catch a thought and Ember will take it from there"); if Threads exist but all rest, one gentle Shelf link ("Your shelf is resting — browse it anytime").
- **Loading / regenerating:** only during dial-change regeneration — previous card grayed with a one-line shimmer (<5 s target). Never a blank spinner at wake; pre-gen guarantees readiness.
- **Notification-silenced:** pings off after 5 consecutive ignores; the Doorway still generates daily. Card carries a quiet one-liner: "We've stopped pinging you — the card is here whenever you want it. Turn pings back on?"
- **Card-skipped:** a non-state by design. Tomorrow's card regenerates as if today went fine. No "you missed yesterday," no streak, no visual debt.
- **Offline:** last generated card served from local cache with a subtle "as of last night" stamp; deterministic actions (skip, dial, snooze) queue and sync later.
- **Error (pre-gen failed):** deterministic fallback card — see §10; the user never sees an error state at wake.

## 7. Workflows

**Happy path — morning open (WF-D1, wireframes DW-01 → DW-03)**
1. 03:00 local: nightly Batch job writes today's card at the remembered dial position.
2. 08:15 (inside her chosen window): ONE notification fires, copy drawn from the varied pool.
3. Maya taps it → card renders instantly from cache (DW-01); ignore counter resets to 0.
4. She reads the warm-threads line, glances at the resurfaced Spark, taps "Do the Pebble."
5. The Thread opens with its Warm Start whisper (M3, WS-01); the Doorway is done in under 90 seconds.
6. Pebble completion feeds tomorrow's generation and the deterministic micro-reward layer.

**Failure path A — hard morning (WF-D2, DW-01 → DW-04)**
1. The normal card feels like too much; she flips the dial to low-spoon.
2. Card regenerates in ≤5 s into the complete one-tiny-thing plan (DW-04) — no trace of the fuller card.
3. She does the tiny thing, or taps "Not today." Either way tomorrow pre-generates at low-spoon.
4. No apology, no comparison, no record shown of what the fuller card would have been.

**Failure path B — three silent weeks (WF-D3, DW-05)**
1. Days 1–5 of absence: scheduled pings fire, get ignored; at the 5th consecutive ignore the engine self-silences and queues one gentle check-in.
2. Days 6–21: fully quiet. No escalation, no "we miss you" pressure.
3. Day 22: she opens the app → welcome-back Doorway (DW-05): "Welcome back. Nothing is lost. Here's what's still warm." + 2 warm Threads + one tiny step; billing-pause line if the 45-day auto-pause fired.
4. "Show me" → Shelf Warm row (M2) or the top Thread's full Briefing (M3). Normal daily cards resume the next morning at her remembered dial position.

**Evening close — optional (WF-D4, DW-06)**
- If she opens Ember within 2 h before quiet-hours start, an optional one-line close appears: "Today's card is done with you — anything to drop off before tomorrow?" with a capture field.
- Anything untouched rolls over silently — nothing "moves to overdue"; tomorrow's card simply regenerates fresh. The close never notifies; it exists only in-app and is skippable like everything else.

**Night capture routing (WF-D5, with M1)**
- Sparks captured 22:00–06:00 via night mode file normally but carry the `night_captured` flag.
- They become priority candidates for the resurfaced-Spark slot on the NEXT morning's card ("You had a thought at 1 am — it's safe here").
- Nothing pings at night, ever: the quiet-hours guard is evaluated at send time, deterministically.

## 8. AI behavior

- **Triggers:** nightly Batch job (per-user, local ~03:00) pre-generates the Doorway Card so it is ready at wake; on-demand regeneration on dial change; welcome-back composition on first open after ≥7 days.
- **Inputs:** per-Thread Digests (never raw history), Thread warm/resting states, next Pebbles from active Arcs (M4), night-captured and resurface-eligible Sparks, current dial position, last 7 cards (framing-variety check), days-since-last-open, deadline lead-time flags.
- **Model tier:** Sonnet-tier for card composition and welcome-back copy; nightly runs on the Batch API (50% off); drops to Haiku-only past the $5/mo spend killswitch.
- **Notification copy pool:** ~2 weeks of varied notification lines pre-generated in the same Batch run and stored per user. Send time never calls a model — the deterministic scheduler draws the next unused line from the pool (anti-habituation via variation, per §1 evidence). Pool exhaustion falls back to a curated static set, still rotated.
- **Prompt contract:** warm second person; at most 3 items; exactly one Pebble and at most one Spark; low-spoon output must read as a complete plan of one tiny thing; opening line must differ from the last 7 cards; forbidden vocabulary enforced (no "task list," "to-do," "overdue," "backlog," "streak"; zero guilt).
- **Output contract:** strict JSON — `{warm_line, pebble_ref, pebble_framing, spark_ref?, spark_action, opening_variant_id}`. Refs are validated deterministically against real IDs before render; an invalid ref drops that slot and the card still renders.
- **Guardrails:** never invents Threads/Pebbles/Sparks; the renderer hard-caps items at 3 regardless of model output; never mentions dial history or days missed; welcome-back never enumerates a backlog.
- **Low-confidence / AI-unavailable fallback:** deterministic card from cached data (§10). The product never blocks on live inference at wake time.
- **Deliberately NOT AI:** notification scheduling, the 2/day hard cap, quiet hours, the self-silencing counter, dial mechanics and persistence, skip/rollover logic, night-capture routing, billing-pause detection. All deterministic and inspectable — no model ever decides *when* to interrupt the user.

## 9. Scale

- The card is O(1) by design: always ≤3 items whether the user has 5 Threads or 500. The surface never paginates because it never grows.
- At 10×/100× data, cost concentrates in candidate selection, which is deterministic pre-filtering: warm Threads and resurface-eligible Sparks scored by recency, deadline proximity, and dial position; only the top ~20 candidates are passed to the model.
- Token input stays bounded at any corpus size because generation reads Digests, never raw history (digest-first architecture, R6).
- Notification copy pool is constant-size per user; card archive is pruned per §4 retention rules.
- Performance budgets: card render from cache <200 ms; dial regeneration <5 s p95 (deterministic fallback served at 8 s); nightly Batch completion before the earliest wake window in each timezone cohort, monitored as an SLO.

## 10. Errors

- **Pre-gen failure (Batch job errored / output invalid):** morning open serves a deterministic fallback card assembled from cached data with template copy: warmest Thread by last-touch, its stored next Pebble (from the Arc, no AI), and the most recent night-captured or resurface-queued Spark. Same 3-slot shape; user never sees an error at wake. Retry once on open in background; if it succeeds, tomorrow improves — today's card does not swap out from under the user.
- **Timezone change (travel/DST):** scheduler keys off device-reported local time; on timezone delta, today's already-generated card is kept (content is date-scoped, not hour-scoped), notification slots re-anchor to the new local window, and the quiet-hours guard re-evaluates before any queued ping fires — a ping that would now land inside 22:00–06:00 local is dropped, not delayed to a weird hour. Next Batch run uses the new zone. Rapid multi-zone hops: at most one card and ≤2 notifications per calendar date, whichever zone.
- **Dial-change regeneration failure:** deterministic degradation-free fallback — low-spoon template card from cached Pebble data; toast "Ember will polish this overnight."
- **Sync conflict (dial set differently on phone and web):** last-write-wins by timestamp; card regenerates once; no prompt.
- **User mistakes:** skip is inherently consequence-free; snoozes and Spark actions undoable via toast; notification re-enable is one tap from the check-in.
- **Notification delivery failure (OS-level):** no retry storm — the card is the source of truth; missed pings never queue up.

## 11. Permissions

Doorway data (dial history, ignore counts, card content) is private to the user; no sharing surface exists in M5. Dial position and silencing state are treated as health-adjacent signals: never exported, never used in marketing/analytics beyond aggregate anonymized counts, never shown back to the user as a trend ("you've been low-spoon 12 days" is forbidden by contract). Notification payloads keep content minimal on the lockscreen (no Thread names by default; user can opt in to fuller previews). Billing-pause note reads billing state but M5 writes nothing to billing.

## 12. Dependencies

- **Consumes:** M1 Catch (`spark.captured` incl. `night_captured` flag), M2 Threads & Shelf (warm/resting states, Digest read), M3 Warm Start (Briefing hand-off targets; whisper on Pebble-tap), M4 Year Arc (next Pebble per active Arc, deadline lead-time flags), cross-cutting billing (auto-pause state), platform push services, nightly Batch pipeline (shared with Digest maintenance).
- **Emits:** `doorway.card_viewed / card_skipped / dial_changed / pebble_accepted / spark_actioned / notifications_silenced / welcome_back_shown` — consumed by M3 (briefing context), M4 (re-planning signals), micro-reward layer (deterministic, sub-300ms), and eval telemetry (R6: briefing/card quality).
- **Requires:** local-time scheduler service (deterministic), cached-card store on device, notification copy pool store, spend killswitch service (Haiku-only mode).
