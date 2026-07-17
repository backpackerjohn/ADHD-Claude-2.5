# Module: M3 — Warm Start

> Crown jewel (per `contest/winner.md`). Terminology and object states follow
> `product/product-brief.md` (single source of truth). Risks owned here: R2, R4, R6.

## 1. Purpose

Warm Start eliminates project re-entry friction — the contest-winning, verified-underserved wedge. Returning to anything after time away feels like starting from zero: "you open the document and can't remember where you were or what you were thinking" ([Fabric](https://fabric.so/blog/how-to-finish-projects-when-your-brain-keeps-starting-new-ones)), and for ADHD working memory "restarting after an interruption may feel like beginning the whole task again… harder than starting something new" ([ADHD Philadelphia](https://www.adhdphiladelphia.com/blog/why-adults-with-adhd-lose-momentum-so-easily-after-interruptions)). The real-world cost is measured in years, not minutes ([ADHDandMarriage](https://www.adhdmarriage.com/content/2-12-years-w-no-kitchen-endless-unfinished-projects-homehelp-me-understand)). When a user opens a Thread after a gap, Warm Start delivers a briefing scaled to time away — where you were, what you were thinking (your own words), why you cared, what changed, one tiny Pebble — and terminates every briefing in action, because memory without execution is the organized-graveyard death (risk R4). It also hosts the **Unstick** interaction absorbed from Ignition (binding synthesis, `contest/winner.md`), delivering help at Barkley's point of performance ([Barkley factsheet](https://www.russellbarkley.org/factsheets/ADHD_EF_and_SR.pdf)).

## 2. User goals

- I want to open a project after weeks away and know where I was in under a minute, so restarting stops feeling like starting from zero.
- I want to hear my own past reasoning in my own words, so I trust the briefing and reconnect with why I cared.
- I want exactly one tiny next step — never the wall of steps — so I can restart momentum, not attempt a finish.
- I want honorable exits (shrink, retire) so a stalled project becomes history, not shame ([Heal & Thrive](https://heal-thrive.com/adhd-and-shame-spirals-how-one-bad-day-becomes-a-week-of-avoidance/)).
- When I freeze on a step, I want bounded help getting unstuck right there, without a therapy chat.

## 3. Objects

- **Briefing** (owned): `tier` (whisper | brief | full), `gap_days`, `sections[]`, `quoted_spark_ids[]`, `confidence`, `state`, `generated_at`, `digest_version_used`.
- **Closing Note** (owned): `thread_id`, `body`, `confirmed_at`; kept in Finished & Retired gallery.
- **Unstick session** (owned, ephemeral): `pebble_id`, `exchange_count` (max 3), `named_feeling`, `shrunk_stake`, `physical_first_move`; transcript retained as thread history.
- **Touched, not owned:** Thread (reads state + last-touched), Digest (sole AI input), Spark (verbatim quote source), Breadcrumb (fallback + "where you were"), Pebble (output; M4 Arc supplies candidates), Arc (Shrink renegotiates scope), Doorway Card (welcome-back link).

## 4. Lifecycle

**Briefing**
- Created: on trigger (thread-open after gap, resting-thread tap, welcome-back link). Never pre-pushed to the user; nightly Batch may pre-warm content silently.
- States: `generated → acted | snoozed` (brief's canonical states). Module-internal sub-outcomes of `acted`: did-pebble, shrunk, retired.
- A snoozed Briefing expires when the Thread is next opened — regenerated fresh, because the gap has changed.
- Cached 24h per Thread (a returning user re-opening the same Thread sees the same briefing, no double spend).
- Retention: last 5 Briefings kept per Thread (for quote-dispute review); older ones deleted. Thread deletion hard-deletes all.

**Closing Note**
- Created: only via Retire with honor. Drafted by AI → optionally edited by the user → **explicitly confirmed** → stored with the retired Thread in the Finished & Retired gallery.
- Un-retiring the Thread preserves the note as an immutable log entry (history is never erased, only continued).
- Deleted only when its Thread is deleted.

**Unstick session**
- Created: Unstick tap on any Pebble. Lives for at most 3 exchanges, then closes — always; the hard cap is structural, not a soft limit.
- Never resumable; a new freeze starts a new session. Transcript is appended to the Thread's story (and the eval pipeline).
- Free tier: 3 Unsticks/day (business constraints, product brief).

## 5. Actions

| Action | Trigger surface | Input | Effect | Undo |
|---|---|---|---|---|
| Open briefing | Opening a Thread after a gap; tapping a resting Thread on the Shelf; "welcome back" link on Doorway Card | tap | Renders tiered Briefing (WS-01/02/03) | dismissible |
| **Do it now** | Briefing button | tap | Opens focus surface (WS-04) with the Pebble; Pebble → accepted | Back exits; auto-Breadcrumb writes on exit |
| **Snooze** | Briefing button | tap (+ optional "how long?" chips: few days / next week / whenever) | Briefing → snoozed; Thread stays where it is (warm stays warm); no reminder debt, no badge | reopen Thread anytime |
| **Shrink** | Briefing button | tap → scope sheet (WS-05) | Renegotiates Arc scope with M4 ("novel" → "novella"; "whole room" → "one wall"); Arc → re-planned; new smaller Pebble offered | revert scope from Arc history |
| **Retire with honor** | Briefing button | tap → Closing Note draft → **explicit confirm** | Thread → retired; AI Closing Note ("You built the hard part. It taught you resin casting.") saved to Finished & Retired gallery | un-retire from Shelf; note preserved |
| **Unstick** ("why is this hard?") | Available on any Pebble — briefing, focus surface, Doorway | tap | Bounded 3-exchange script (WS-06), see §8 | close anytime |
| Mark Pebble done | Focus surface | tap | Pebble → done; deterministic micro-reward <300ms, novelty-rotating; ember hours accrue | un-mark within session |
| "That's not what I meant" | Long-press any quoted line | flag + optional correction | Quote suppressed from future briefings; feedback logged to eval set | unflag in settings |

## 6. States

- **First-ever briefing (thin corpus):** Thread has <3 Sparks and no Breadcrumbs. No fabricated warmth: "This thread is young — here's everything I have," shows the raw Sparks verbatim plus one suggested Pebble. Sets expectations honestly (R6: never fake richness).
- **Whisper (gap <3 days):** one line inline at the top of the Thread, not a screen: "Yesterday you stopped mid-email to the accountant — the deduction question." No buttons except the Pebble chip.
- **Brief (gap 3–13 days):** compact card (WS-02): where you were + last Breadcrumb + one Pebble + the four buttons.
- **Full (gap ≥14 days):** full-screen warm briefing (WS-03), second person, tone "Welcome back. Nothing is lost." Five sections in fixed order:
  1. **Where you were** — last actions and the final Breadcrumb, narrated.
  2. **What you were thinking** — verbatim Spark quotes, visually styled as quotes with dates: your past self talking to you.
  3. **Why you cared** — the Thread's origin motivation, from the Digest.
  4. **What changed while you were gone** — deadlines moved, related Sparks arrived, Arc re-plans.
  5. **One tiny Pebble** — a single next step sized to restart momentum, never a step list.
  Followed by the four buttons: **Do it now · Snooze · Shrink · Retire with honor.**
- **Low-confidence / insufficient memory:** grounding check failed or Digest too sparse for the gap. Shows "I don't have enough memory of this thread yet" + raw thread story + last Breadcrumb + generic small Pebble ("re-read your last three notes"). Never guesses.
- **AI unavailable (offline / outage):** deterministic fallback — raw thread story (chronological Sparks, steps, Breadcrumbs from M2) with last Breadcrumb pinned on top. Product remains usable; briefing marked "Ember's memory is resting — here's the raw story."
- **Snoozed:** Thread shows a quiet "briefing resting" glyph; no countdown, no red.
- **Retired:** Thread shows its Closing Note; reopening offers "pick this back up?" (un-retire).
- **Loading:** skeleton card with the Thread name and gap ("You've been away 5 weeks — warming this up…"); target <4s, else fallback per §10.

## 7. Workflows

**Happy path — full re-entry after 6 weeks (Shelf → WS-03 → WS-04):**
1. Maya taps her resting "Etsy shop" Thread on the Shelf (or the welcome-back Doorway link, which deep-links to the same place).
2. Loading skeleton with honest copy: "You've been away 6 weeks — warming this up…" (<4s).
3. Full briefing (WS-03) renders: where she was (from Breadcrumbs), what she was thinking (two verbatim Sparks from March, dated and quote-styled), why she cared, what changed while she was gone (the craft-fair deadline moved), and one Pebble: "open the shop banner file and just look at it (2 min)."
4. She taps **Do it now** → focus surface (WS-04): only the Pebble, the Thread's key links, and a capture field. No other UI.
5. She works 20 minutes, marks the Pebble done → deterministic micro-reward fires in <300ms (novelty-rotated variant) → ember hours accrue.
6. She leaves mid-flow (as ADHD focus breaks do) → auto-Breadcrumb writes itself on exit → tomorrow's Doorway knows.

**Failure path 1 — freeze on the Pebble (WS-04 → WS-06):**
1. She reads the Pebble and freezes — the wall of awful ([ADHD Essentials](https://www.adhdessentials.com/essentials/the-wall-of-awful/)).
2. Taps **Unstick** ("why is this hard?") → exchange 1: name the feeling (chips + free text).
3. Exchange 2: shrink the stakes ("this is a look, not a launch — nothing you do today is graded").
4. Exchange 3: one 2-minute physical first move ("open the laptop and put the banner file on screen, that's all"). Session closes — no fourth turn exists.
5. Two buttons: start the 2 minutes / not today. "Not today" is accepted without guilt; the Thread rests, nothing turns red.

**Failure path 2 — the briefing lands wrong (WS-03 → WS-05):**
1. The briefing quotes a Spark whose framing she now disputes. Long-press → "that's not what I meant" → optional one-line correction.
2. Quote suppressed thread-wide; feedback logged to the eval set; briefing offers a one-time regenerate.
3. If instead she realizes the project itself is dead: **Retire with honor** → AI drafts the Closing Note ("You built the hard part. It taught you resin casting.").
4. She edits one line, taps **confirm** (explicit — retirement never happens on one tap) → Thread → retired, note shelved in the Finished & Retired gallery. Graveyard becomes history.

## 8. AI behavior

- **Trigger:** on-demand only (pull-based, at the point of performance — the anti-Mem lesson). Never push-generated except pre-warm when the nightly Batch job sees a welcome-back Doorway is due.
- **Inputs (digest-first, R6):** the Thread's maintained **Digest** — never raw history — plus last 3 Breadcrumbs, candidate Pebbles from the Arc (M4), gap length, and top-k Sparks retrieved for quotation. Prompt caching on the stable system prefix.
- **Model tier:** Sonnet-tier for briefings and Closing Notes; Haiku-tier for whisper one-liners and Unstick exchanges; killswitch drops all to Haiku past $5/mo/user. Cost per full briefing ≈ $0.026 (thesis-B §8: 20/mo ≈ $0.51).
- **Output contract:** structured sections + `quoted_spark_ids[]`; tone warm, second person, zero guilt; exactly one Pebble, sized 2 min–1 hr to restart momentum, not finish (the middle-60% dead zone, [Tiimo](https://www.tiimoapp.com/resource-hub/finishing-what-you-start-adhd)).
- **Grounding rule (hard, R6):**
  - Every quoted line must byte-match a real stored Spark. A post-generation verifier (deterministic code, not AI) checks each `quoted_spark_id` against the corpus.
  - Mismatch → strip the offending quote; if the briefing can't stand without it → regenerate once → else render the insufficient-memory state.
  - Every factual claim in "where you were / what changed" must trace to Digest or Breadcrumb content; the model is instructed to omit, never invent.
  - **Never hallucinate memory. The fallback is always honest: "I don't have enough memory of this thread yet."** A thin briefing that admits thinness beats a rich briefing that lies — briefing quality is the product (risk R6).
- **Unstick contract (absorbed from Ignition — bounded, NOT a chat, hard-capped at 3 exchanges):**
  1. *Name the feeling* — chips + free text: "which is closest — too big / too boring / scared it'll be bad / don't know where to start?"
  2. *Shrink the stakes* — reframes the step as an unmeasured draft/trial, not a performance.
  3. *2-minute physical first move* — concrete, bodily, timeboxed ("stand up and open the paint can").
  - No fourth turn exists in the UI; there is no free-form chat surface anywhere in the flow (chat is a blank-page decision — thesis-B §6).
  - Transcripts double as golden-set prompt-eval data (winner.md steal #5).
- **Quality harness:** golden-set evals seeded from Wizard-of-Oz concierge transcripts; "that's not what I meant" flags continuously feed the set; ship gate mirrors the concierge threshold — ≥50% of briefings rated "I could restart from this alone" (thesis-B §9).
- **Deliberately NOT AI:**
  - Gap computation and tier selection (pure date math).
  - Snooze handling and briefing cache/expiry.
  - Micro-rewards: deterministic, fired in <300ms from a pre-built novelty-rotating pool, kept entirely out of the API round-trip — delay-discounting compliance ([JAD meta-analysis](https://journals.sagepub.com/doi/10.1177/1087054718772138)); rotation counters habituation ([Brain, novelty processing](https://academic.oup.com/brain/article/141/5/1545/4934119); alarm-blindness evidence, [My Patient Advice](https://mypatientadvice.co.uk/knowledge-base/why-do-adhd-brains-still-ignore-phone-alarms/)).
  - Ember-hours accrual, retire confirmation, notification scheduling, billing.

## 9. Scale

- **10× (dozens of Threads, a year of history):** Digest-first keeps briefing input bounded (~6K tokens) regardless of Thread size; quote retrieval is top-k over embeddings, O(corpus) handled by M2 search infra. Briefing latency budget unchanged (<4s p90).
- **100× (heavy importer: 4,000-note graveyard, multi-year Threads):** Digests are hierarchical (era summaries roll up); "what changed" section windows to since-last-touch only; per-Thread briefing cache prevents regeneration storms when a returning user opens ten Threads in one sitting; retired Threads' Digests are frozen (no nightly maintenance cost). Briefing history pagination: last 5 kept, rest pruned.
- Performance budgets: whisper line renders from cache instantly (pre-computed nightly); full briefing p50 <2.5s, p90 <4s, hard timeout 8s → fallback state.

## 10. Errors

- **Generation failure / timeout:** one silent retry (8s hard timeout) → AI-unavailable fallback (raw thread story + last Breadcrumb). Never a blank screen; never "try again later" as the only content — the user came here to re-enter, and re-entry must always be possible.
- **Stale digest:** briefing detects `digest_version < last_activity` (e.g., last night's Batch missed new Sparks).
  - Small delta → generate from Digest + the raw items since the digest timestamp.
  - Large delta (heavy capture burst, fresh import) → "catching up on your latest notes…" and run an inline digest refresh first; fall back to raw story if that fails too.
- **User disputes a quote ("that's not what I meant"):**
  - Quote suppressed thread-wide immediately; optional correction stored alongside the original Spark (the Spark itself is never edited).
  - Dispute logged to the eval pipeline (R6 harness); one regenerate offered.
  - Repeated disputes on a Thread lower its briefing confidence → future briefings prefer showing raw Sparks over AI paraphrase for that Thread.
- **Accidental retire:** explicit confirm on the flow; un-retire from Shelf restores warm/resting state; Closing Note preserved as history.
- **Sync conflict (two devices):** Briefings are device-local ephemera — regenerate, never merge. Pebble done/state conflicts resolve last-write-wins with union of Breadcrumbs (no data loss).
- **Unstick misuse (user keeps reopening):** each session is fresh and capped at 3 exchanges; after 3 sessions on one Pebble in a day, offer Shrink instead ("this step might just be too big").

## 11. Permissions

- Briefings, Unstick transcripts, and Closing Notes derive from the user's private corpus; visible to the owner only. No sharing surface in v1.
- ADHD is health-adjacent data: Unstick `named_feeling` entries are the most sensitive field in the product — encrypted at rest, excluded from analytics events (only counts, never content), never used for model training without explicit opt-in.
- Deleting a Thread hard-deletes its Briefings, Digest, and Unstick transcripts. Auto-pause of billing (45 days idle) never deletes memory — "everything is safe."

## 12. Dependencies

- **Consumes:** M2 Digest (sole synthesis input) + thread story (fallback) + last-touched timestamps; M4 Arc for candidate Pebbles and Shrink renegotiation; M5 Doorway welcome-back link (entry) ; M1 Sparks (quote source); cross-cutting reward layer + ember hours; nightly Batch (digest freshness, whisper pre-compute, welcome-back pre-warm).
- **Emits:** `briefing_generated`, `briefing_acted/snoozed`, `pebble_accepted/done` (→ Doorway's next card, ember hours), `thread_shrunk` (→ M4 re-plan), `thread_retired` + Closing Note (→ Shelf Retired gallery), `quote_disputed` (→ eval pipeline), `unstick_completed`.
- **Services:** Sonnet/Haiku-tier inference, embedding retrieval, grounding verifier, prompt cache. Degrades gracefully without all of them (§6 AI-unavailable).
