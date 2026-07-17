# Ember — Shared Product Brief (Single Source of Truth)

> Every later artifact (module specs, AI spec, wireframes, prototype, recap) MUST use the terminology, module list, and object model defined here. Do not reinvent. Changes require editing this file first and noting the change in build-log.md.
> Basis: `contest/winner.md` (unanimous verdict + binding synthesis). Evidence: `research/`.

## One-liner

**Ember — the app that keeps your projects warm.** Capture any thought in two seconds with zero decisions; Ember files it, remembers everything, and when you come back to anything — after a day or after two months — it warmly briefs you back in: here's where you were, here's what you were thinking, here's the one tiny next step.

## Target user

**Maya, 34, diagnosed with ADHD at 31** — one of the ~half of 15.5M diagnosed US adults diagnosed in adulthood ([CDC](https://www.cdc.gov/mmwr/volumes/73/wr/mm7340a1.htm)). Multi-project adult: side projects, creative work, life-admin arcs (taxes, job hunt, a move). Owner of a project graveyard (novel at 60%, half-built Etsy shop, two-color room). Has abandoned Notion, Apple Notes, and planners. Cannot afford a $170–225/hr coach. Wants to stop losing the plot of her own life. Platform reality: phone-first (capture), web/desktop for project work.

## Winning problem (contest-approved)

**Project re-entry friction + upstream thought/context loss.** Returning to anything after time away feels like starting from zero ("you open the document and can't remember where you were or what you were thinking" — [Fabric](https://fabric.so/blog/how-to-finish-projects-when-your-brain-keeps-starting-new-ones); "restarting harder than starting something new" — [ADHD Philadelphia](https://www.adhdphiladelphia.com/blog/why-adults-with-adhd-lose-momentum-so-easily-after-interruptions)). Verified as the only confirmed-and-underserved wedge (`research/verification.md`). Honesty clause: frequency/severity qualitatively convergent, not yet quantitatively measured; month-one validation plan exists (thesis-B §9).

## Product thesis

The daily verb is **depositing** (capture); the payoff verb is **returning** (Warm Start). Ember is designed for absence: when the user disappears — and they will (30-day category retention ≈3.3%, [Baumel 2019](https://www.jmir.org/2019/9/e14567/)) — its value grows. No streaks, no overdue debt, no red. Execution support is delivered at the point of performance (every surface ends in one tiny step; an unstick interaction exists at the moment of freeze). Honest billing is a feature.

## The five modules (fixed list — do not add or rename)

| ID | Name | Purpose | Founder mandate |
|---|---|---|---|
| **M1** | **Catch** | Zero-decision capture (voice/text; widget, share sheet, night mode, web) → AI auto-files into Threads; visible "Loose Sparks" tray for low-confidence items; one-drag correction; graveyard import (Apple Notes/Notion, cost-capped) | #1 organize thoughts |
| **M2** | **Threads & Shelf** | Living memory. Every capture/step/decision/breadcrumb accrues to a Thread (project, idea cluster, life-admin arc, person). Shelf = browse/find surface: Warm row, Resting shelf (amnesty — paused ≠ failed), Finished & Retired gallery; hybrid search (semantic + keyword + time) | #3 view, pick up, find |
| **M3** | **Warm Start** | Crown jewel. Re-entry briefing scaled to time-away (one-liner after a day; full brief after weeks): where you were, what you were thinking (your own words quoted), why you cared, one tiny next step. Four buttons: **Do it now · Snooze · Shrink · Retire with honor** (AI closing note). Includes the **Unstick** interaction (from Ignition): "why is this hard?" → name the feeling → shrink the stakes → 2-minute physical first move | — (execution layer) |
| **M4** | **Year Arc** | Year-scale breakdown that survives months: big thing → milestones → next **Pebble** only (never the wall of steps); context-aware re-planning when reality changes; deadline arcs get backwards-planned lead time; calendar-read felt-time triggers (v1 phase 2: "leave by 1:15") | #2 break down big tasks, workable all year |
| **M5** | **Doorway** | Bounded daily card (≤90 seconds, ≤3 items): warm threads, one suggested Pebble, one resurfaced Spark, **capacity dial** (from Anchor: a low-capacity day renders a complete plan, not a degraded one). Anti-habituation notifications (varied copy, self-silencing, capped). Night-safe capture routes here. Welcome-back Doorway after absence | — (daily anchor) |

**Cross-cutting systems (not modules):** deterministic sub-300ms micro-reward layer with novelty rotation; cumulative streak-free progress ("ember hours"); auto-breadcrumb on exit (zero user effort); honest billing (two-tap cancel, trial-end warning, auto-pause after 45 days idle); privacy-first data handling.

## Object model (canonical nouns)

| Object | Definition | Key states |
|---|---|---|
| **Spark** | A single captured thought (text or voice+transcript) | unfiled (Loose) → filed → resurfaced / archived |
| **Thread** | The living container: project, idea cluster, life-admin arc, or person | warm · resting · finished · retired |
| **Pebble** | One tiny next step attached to a Thread (2 min – 1 hr, sized to restart momentum) | suggested → accepted → done / dissolved (never "overdue") |
| **Arc** | A Year-Arc plan on a Thread: milestones + generated Pebbles + optional deadline | active · re-planned · paused · complete |
| **Breadcrumb** | Auto-written note of where/why the user stopped | (immutable log entries) |
| **Digest** | AI-maintained per-Thread summary (the context read by briefings — never raw history) | continuously updated |
| **Briefing** | A Warm Start output (scaled: whisper &lt; 3 days; brief ≥ 3 days; full ≥ 14 days) | generated → acted / snoozed |
| **Doorway Card** | The daily bounded surface | today's · welcome-back |
| **Closing Note** | AI-written honorable retirement note for a Thread | (kept in Retired gallery) |

**Terminology rules:** never "task list", "to-do", "overdue", "streak", "backlog", "inbox" (use Loose Sparks), never red badges. Tone: warm, second person, zero guilt ("Welcome back. Nothing is lost.").

## Platforms

- **Mobile app (iOS/Android)** — capture-first: widget, share sheet, night mode, Doorway, quick Warm Starts.
- **Web app (desktop)** — memory-first: Shelf, full Thread stories, Arc planning, search, import, settings/billing.
- Shared design system; platform-specific layouts (see design/).

## AI architecture (summary — full spec in ai-spec.md)

Haiku-tier for filing/triage/digests (workhorse); Sonnet-tier for Doorway, Briefings, Arc breakdown; nightly Batch jobs (digests, Doorway pre-gen); prompt caching on stable prefixes; spend killswitch → Haiku-only past $5/mo/user. Deterministic (never AI): reminders/notification scheduling, deadline math, rewards, progress, billing, capture recording (on-device transcription default). No calendar auto-scheduling. No chat-first UI. Cost at high usage ≈ $3.04–4.75/mo (two independent models: thesis-B §8, product/llm-cost-model.md) — under the $5.99 guardrail.

## Business constraints

Price **$8.99/mo or $79/yr**, 21-day full trial, no card up front; free tier: capture + 3 unsticks/day + 1 thread arc. Gross margin >60% at worst-case AI COGS. No AI phone calls, no hardware, no publishing/integrations beyond calendar read (phase 2) and note import.

## Assumptions & risks (owned)

R1 capture-lane gravity · R2 delay-discounting vs deposit/payoff arc · R3 misclassification vs "zero-decision" trust · R4 organized-graveyard (execution must be real) · R5 one-month v1 scope (no watch/clipper at launch) · R6 briefing quality is the product. Full list + mitigations: `contest/winner.md`. Month-one validation plan: thesis-B §9.

## Definition-of-done hooks

recap.html must let a stranger grasp this in 5 minutes; wireframes must show every module, state, modal, search/filter/bulk/archive/recovery; brand guidelines must be complete enough to make a new on-brand asset; red-team objections must be visible.
