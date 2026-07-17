# Module: M4 — Year Arc

> Conforms to `product/modules/_TEMPLATE.md`. Nouns, states, and terminology per `product/product-brief.md` (source of truth). Synthesis constraints per `contest/winner.md` (calendar-read felt-time triggers are **phase 2 of v1**).

## 1. Purpose

Year Arc is the breakdown that survives months — founder mandate 2 ("break down big tasks, workable all year"). Big things die for two evidenced reasons: at the start, "all the steps involved in a project tend to merge into one big, intimidating task" ([ADDitude](https://www.additudemag.com/where-do-i-start-adhd-organization/)); in the middle, existing breakdown tools are one-shot — Goblin Tools "generates a beautiful list of steps and then that's it… a planning tool, not an execution tool" ([Thawly](https://thawly.ai/reviews/goblin-tools)) with "no personalization… each session starts fresh" ([FocusHack](https://www.focushack.io/reviews/goblin-tools-adhd-review/)). M4 turns a Thread into an **Arc**: AI-decomposed milestones across months, always rendered as exactly one next **Pebble**, silently re-planned when reality changes, with deterministic backwards-planned lead time for deadlines ([taxes pain: ADDitude](https://www.additudemag.com/a-guide-to-filing-taxes-for-adhd-adults/)) and — phase 2 — read-only calendar felt-time triggers for the unserved "start now to make Y" slice ([Healthline, waiting mode](https://www.healthline.com/health/adhd/adhd-waiting-mode); [ADDitude, time blindness](https://www.additudemag.com/punctuality-time-blindness-adhd-apps-tips/)).

## 2. User goals

- "I want to say 'make this an arc' and get a plan that knows *my* thread — my deadline, what I've already done, when I have energy — so I don't get a generic list."
- "I want to see only the next small step, so the plan never becomes the wall that stops me."
- "I want the plan to quietly fix itself when life happens, so returning after weeks doesn't mean re-planning from scratch ([restarting is harder than starting — ADHD Philadelphia](https://www.adhdphiladelphia.com/blog/why-adults-with-adhd-lose-momentum-so-easily-after-interruptions))."
- "I want deadline pressure translated into gentle, early lead time — not panic, not red."
- "I want missing a date to be a fork in the road, not a failure verdict."
- (Phase 2) "I want the app to feel time for me: 'leave by 1:15 to make the 2:00', and something right-sized to do while I wait."

## 3. Objects

| Object | Ownership | Fields owned here |
|---|---|---|
| **Arc** | Owned by M4 | `thread_id`, `goal_statement`, `milestones[]` (title, target_month, status), `deadline?` (date, source: user), `lead_time_plan?` (deterministic back-schedule), `pace` (gentle/steady/deadline-driven), `energy_notes` (user-stated patterns quoted from captures), `replan_log[]` (diffs), `state` |
| **Pebble** | Shared with M2/M3/M5; M4 generates arc Pebbles | `arc_id`, `milestone_id`, `size_estimate` (2 min–1 hr), `waiting_mode_sized?` (phase 2 flag) |
| **Thread** | Owned by M2; M4 reads/annotates | reads Digest, deadline mentions, done history |
| **Digest** | Owned by M2 nightly jobs | M4 reads it as decomposition context; writes back "arc summary" section |
| **Doorway Card** | Owned by M5 | M4 offers at most **one** arc Pebble per card |

No new nouns are introduced. An "arc proposal" is a **draft Arc** (pre-lifecycle); it is not a separate object.

## 4. Lifecycle

**Arc** (canonical states per brief: `active · re-planned · paused · complete`, plus pre-life `draft`):

- `draft` → created by "Make this an arc"; exists only in the proposal-review screen; discarded silently if never approved (kept 7 days, then deleted). Approval → `active`.
- `active` → normal state; serves next Pebble.
- `re-planned` → transient state after any silent re-plan; user sees the diff once, acknowledges (or ignores for 48 h) → back to `active`. Re-plan diffs append to `replan_log` (retained, capped at last 50).
- `paused` → follows its Thread to the Resting shelf, or user pauses the arc alone. **Deadlines don't sleep:** for a deadline-bearing Arc, deterministic deadline math and lead-time Doorway surfacing CONTINUE while the arc is paused/resting — only AI maintenance pauses. No debt accrues. Resume → re-plan check (the absence ≥14d trigger runs here, §8) → `active` or `re-planned`.
- `complete` → last milestone done or user declares it. Immutable; lives with the Thread in the Finished gallery. Never auto-deleted.
- Deletion: only via Thread deletion (M2 rules). Retiring the Thread (M3) closes the arc as `complete (retired)` with the Closing Note.

**Deadline sub-states** (computed, deterministic — not lifecycle states): `deadline-approaching` (inside lead-time window), `deadline-missed` (date passed with milestones open). These are display conditions on an `active` arc.

**Arc Pebbles** follow the global Pebble lifecycle: suggested → accepted → done / dissolved. Never "overdue" — a Pebble outlived by a re-plan is dissolved silently.

**Allowed transitions (exhaustive):**

| From | To | Cause |
|---|---|---|
| — | draft | "Make this an arc" |
| draft | active | user approval (possibly after edits) |
| draft | (deleted) | discard, or 7-day expiry |
| active | re-planned | silent re-plan (see §8 triggers) |
| re-planned | active | diff acknowledged, or 48 h elapsed |
| active / re-planned | paused | user pause, or Thread → resting |
| paused | active / re-planned | resume (re-plan check decides which) |
| active / re-planned | complete | last milestone done, user declares done, or M3 Retire |
| complete | — | terminal; no reopening (start a new arc on the same Thread instead) |

## 5. Actions

| Action | Trigger surface | Input | Effect | Undo |
|---|---|---|---|---|
| Make this an arc | Button on any Thread (web primary, mobile available) | Optional: deadline, pace preference | AI drafts milestones from thread context → proposal review | Discard draft, no trace |
| Approve arc | Proposal review screen | Optional edits first | Draft → `active`; first Pebble generated | Pause or retire later |
| Edit proposal | Proposal review: rename/merge/delete/re-order milestones, change months, set/clear deadline | Direct manipulation | Edits stored; AI re-balances around user edits (user edits win) | Revert-to-draft button |
| See the whole shape | "View milestones" tap on arc header — **collapsed by default, one tap to expand** | — | Read-only milestone timeline; steps within milestones stay summarized | Collapse |
| Nudge the plan | Arc header menu | Free-text ("I have March off", "push everything a month") | Triggers re-plan → diff view | Diff has "keep old plan" |
| Pause arc | Arc header menu | — | → `paused`, amnesty copy | Resume |
| Re-aim / Shrink / Retire | Deadline-missed prompt; also via M3 buttons | One tap | Re-aim: new date → re-plan. Shrink (M3): smaller goal → re-plan. Retire (M3): close with honor | Diff review before commit |
| Mark milestone done | Milestone view | — | Deterministic; may trigger re-plan of remainder | Un-mark within session |
| Connect calendar (phase 2) | Settings, read-only OAuth | Consent | Enables felt-time triggers | Disconnect purges cache |

## 6. States

- **No-arc thread** (empty): Thread shows a quiet "This looks big. Want an arc?" affordance only when the Digest suggests multi-month scope; never nags.
- **Arc-proposal review (W-09)**: draft milestones + the reasoning in one warm sentence each ("Tax docs first — you mentioned the 1099 email is already found"). Nothing is real until approved.
- **Loading**: "Reading this thread's story…" skeleton; decomposition may take seconds (Sonnet-tier).
- **Ideal (active, W-10)**: arc header = goal + progress warmth ("3 of 7 milestones behind you") + **one next Pebble**. The wall of steps is never the default view — the wall is the documented paralysis trigger ([ADDitude](https://www.additudemag.com/where-do-i-start-adhd-organization/)).
- **Expanded milestone view** (partial): exists for users who want the shape — one tap on the arc header, collapsed again by default on every open. Months render as a horizon (milestone titles + target months), not a checklist; individual steps inside milestones stay summarized as counts ("4 small steps live here").
- **Re-planned diff view**: "The plan bent, it didn't break." Shows only what changed (moved / merged / dissolved), old plan ghosted, one acknowledge tap.
- **Paused**: rests with the Thread; copy: "Paused, not failed. It'll be here."
- **Deadline-approaching**: gentle Doorway surfacing inside the lead-time window; calm copy, no red, no countdown anxiety.
- **Deadline-missed** (amnesty): "The date moved past us — want to re-aim, shrink, or retire?" Three buttons, zero guilt math, no accumulated-overdue display.
- **Complete**: celebration (deterministic micro-reward layer) + offer of a Closing Note for the gallery.
- **Returning after N weeks**: arc opens through M3 Warm Start; if a re-plan happened during absence, the diff is folded into the briefing, not shown as a separate alarm.
- **Error / offline**: existing plan and next Pebble render from local store; "Make this an arc" queues politely (see §10).

## 7. Workflows

**W1 — Happy path: Thread → Arc → months of Pebbles** (wireframes W-M4-01…04)
1. Maya opens her "Taxes" Thread on web, taps **Make this an arc**, adds the April 15 deadline.
2. AI reads the Digest — captures ("I dread the deduction question"), done history (accountant email drafted), stated energy ("I'm useless on weeknights") — and drafts 6 milestones across Feb–Apr (W-M4-02).
3. Proposal review: she renames one milestone, deletes another, approves. Arc → `active`; deterministic back-scheduler computes lead-time checkpoints from April 15.
4. Thread now shows one Pebble: "Find the 1099 email (~5 min)." Doorway may carry it (max one arc Pebble, M5 rule).
5. Weeks pass; Pebbles done/dissolved advance the arc; milestone completions trigger quiet re-balances.

**Failure path A — Decomposition misses the mark:** proposal feels generic or wrong. Mitigation: full inline editing before approval; "try again with a hint" free-text re-prompt; if AI is unavailable or confidence is low, offer a 3-milestone manual skeleton (see §8 fallback). Nothing becomes real without approval — protects R3-class trust.

**W2 — Deadline arc under pressure (deterministic, gentle)** (wireframes W-M4-05…06)
1. Arc "Move apartments" has a hard date (lease end, Aug 31). At approval, the back-scheduler placed lead-time checkpoints: book movers by Aug 1, start packing by Aug 10, utilities by Aug 20 — pure date math, inspectable in the milestone view.
2. On July 28 the arc enters `deadline-approaching` for the movers checkpoint. The Doorway carries the arc Pebble ("Get one mover quote, ~15 min") with calm copy — surfacing earlier and slightly more often within M5's notification caps, never louder. No red, no countdown.
3. (Phase 2) Aug 30, a 2:00 pm walkthrough is on her calendar: the Doorway shows "leave by 1:15 to make the 2:00" and offers one waiting-mode-sized Pebble ("Label the kitchen boxes, ~20 min") for the dead zone before it.
4. Deadline met → milestone celebration; deadline passes with items open → `deadline-missed` prompt: "The date moved past us — want to re-aim, shrink, or retire?" Whatever she picks, the replan_log records it as a decision, not a failure.

**Failure path A' — Panic-escalation temptation (designed against):** deadline proximity never changes tone, only timing. The copy set for deadline-approaching is fixed and pre-written (§8 guardrails); if the user ignores surfacing, notifications self-silence per M5 — the arc simply waits, then offers the amnesty fork.

**Failure path B — Long absence + moved deadline:** Maya vanishes 5 weeks; meanwhile she'd noted "accountant pushed us to May 1." Nightly job re-plans; on return, Warm Start (M3) opens with the brief, then one diff card: "While you were away the plan bent, it didn't break — two milestones slid to April." One tap acknowledges; next Pebble is already sized for restart momentum. No backlog of missed steps is ever shown ([absence is the norm: Baumel 2019](https://www.jmir.org/2019/9/e14567/)).

## 8. AI behavior

- **Triggers:** (a) "Make this an arc" (interactive); (b) re-plan events — arc Pebble done/dissolved beyond threshold, milestone done, deadline changed, user nudge, resume-from-pause; the **absence ≥ 14 days re-plan trigger runs as a resume-time check** (and via nightly Batch for deadline-bearing arcs only — deadlines don't sleep, §4); (c) phase 2: pre-appointment waiting-mode Pebble selection.
- **Silent re-planning semantics:** re-plans run in the background and never interrupt — no push, no badge. The result waits as the one-tap diff view at the user's next natural visit to the arc (or inside the Warm Start brief after absence). "Silent" means the *work* is invisible; the *change* is always disclosed, gently, before the new plan is acted on.
- **Inputs:** Thread Digest (never raw history), open/done Pebbles, user-stated energy patterns and deadline mentions quoted from captures, current milestones + replan_log tail.
- **Model tier:** Sonnet for decomposition and re-planning (judgment + tone); Haiku for cheap re-balance checks ("did enough change to warrant a Sonnet re-plan?"). Nightly re-plans via Batch API. Prompt caching on stable prefixes. Killswitch: past $5/mo → Haiku-only, re-plans become simple deterministic shifts.
- **Prompt contract:** produce 3–9 milestones over the stated horizon; every milestone must have a first Pebble sized 2 min–1 hr; respect user-stated constraints verbatim; warm second person; never emit more than one "next step" for display.
- **Output contract:** strict JSON (milestones, pebbles, one-sentence rationales, diff-vs-previous). Re-plans must output a *minimal* diff, and copy must follow amnesty rules (no "behind", "overdue", "missed" as verdicts).
- **Guardrails:** AI never sets or moves the deadline date; never schedules calendar events; never escalates urgency tone (deadline proximity language is chosen from a fixed, pre-written copy set). User edits are immutable constraints in later re-plans.
- **Fallback:** AI down/low-confidence → deterministic skeleton arc (goal → 3 evenly spaced milestones → user writes the first Pebble) with "Ember will refine this when it's back."
- **Deliberately NOT AI:** deadline math and lead-time back-scheduling; "leave by 1:15" arithmetic (calendar event time − travel/prep buffers = deterministic); notification timing; progress display; celebration. Per brief: no calendar auto-scheduling, ever ([Motion's oppressive-schedule failure](https://www.saner.ai/blogs/motion-reviews)).
- **Phase 2 (felt-time, read-only calendar — winner.md steal #7):** deterministic scan of today's events → Doorway/notification line "leave by 1:15 to make the 2:00" (pure math); AI's only role is choosing a **waiting-mode-sized Pebble** (≤ the free window, low-stakes, physical-first) to offer before appointments ([Sagebrush](https://www.sagebrushcounseling.com/blog/what-is-adhd-waiting-mode)). Ships week 4 of v1 or is cut (R5).

## 9. Scale

- **Budgets:** 20 concurrent arcs per user; year-long horizon per arc; 200-Pebble history per arc (older done/dissolved Pebbles roll into the Digest and a compact archive — history view paginates 25 at a time).
- **10×:** milestone timeline virtualizes; replan_log capped at 50 diffs (older summarized into one line); nightly re-plan checks batch all arcs in one job per user.
- **100× (long-lived account):** completed arcs are cold storage — rendered from stored summary, no AI reads; search over arc history goes through M2 hybrid search, not M4.
- **Performance:** arc header + next Pebble render < 200 ms from local store; decomposition is the only slow path and is explicitly framed as "reading your thread."
- **Cost:** ~15 breakdown/re-plan calls/mo ≈ $0.32 at P90 (thesis-B §8) — within the $5.99 guardrail.

## 10. Errors

- **AI failure mid-decomposition:** no partial writes — draft is atomic; on failure, offer retry or manual skeleton. A failed re-plan leaves the previous plan untouched (`active`, unchanged).
- **Sync conflict** (edited proposal on web while mobile re-planned): last-writer-wins on distinct fields; conflicting milestone edits surface a single gentle chooser ("two versions of this milestone — keep which?"). User edits always beat AI output.
- **Deadline typo / user mistake:** deadline edits show the recomputed lead-time before commit; every re-plan diff has "keep old plan"; milestone deletes are undoable for 30 days via replan_log.
- **Calendar read failure (phase 2):** felt-time lines silently absent — never a stale "leave by" time; degraded is silent, not wrong.
- **Offline:** plan and next Pebble are local-first; arc creation queues with clear "will draft when back online" copy.

## 11. Permissions

- Arcs are private to the account; no sharing in v1. ADHD is health-adjacent data: goal statements and energy notes are treated as sensitive content (encrypted at rest, excluded from analytics payloads; only anonymized event counts leave the device/server boundary).
- Calendar (phase 2): **read-only** OAuth scope, event times/titles only, processed transiently for same-day math, never stored beyond the day, disconnect purges cache immediately, plainly explained at consent.
- Free tier: **1 thread arc** (per brief business constraints); paywall copy is honest about the limit, no dark pattern.

## 12. Dependencies

- **M2 Threads & Shelf:** hosts the Arc on its Thread; supplies the Digest (decomposition context); Resting shelf drives `paused`; Finished gallery displays `complete` + Closing Note.
- **M3 Warm Start:** the return surface. **Shrink renegotiates the arc** (M3 Shrink → M4 re-plan to a smaller goal); **Retire closes it** (Closing Note honors what the arc taught). Absence-period diffs are delivered inside the briefing.
- **M5 Doorway:** consumes at most **one arc Pebble per Doorway card**; deadline-approaching surfacing and (phase 2) felt-time lines render only inside Doorway/notification bounds (capped, self-silencing).
- **M1 Catch:** captures mentioning dates/energy accrue to the Thread and feed re-planning context.
- **Cross-cutting:** deterministic micro-reward layer (milestone/complete celebrations); auto-breadcrumb (re-entry context); notification system (deterministic scheduling only).
- **Events emitted:** `arc.created/approved/replanned/paused/resumed/completed`, `arc.deadline_window_entered`, `arc.deadline_passed`, `arc.pebble_suggested`.
- **Events consumed:** `pebble.done/dissolved`, `thread.paused/retired`, `capture.filed(thread)`, `calendar.day_events` (phase 2), `user.returned_after_absence`.
