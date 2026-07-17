# Thesis B — Project-Memory-First

**Team B. Strategic angle: the product is the user's external memory for projects and thoughts, and its crown jewel is re-entry.**

---

## 1) Product name + one-line pitch

# **Ember** — *the app that keeps your projects warm.*

Capture any thought in two seconds with zero decisions; Ember files it, remembers everything, and when you come back to anything — after a day or after two months — it warmly briefs you back in: *here's where you were, here's what you were thinking, here's the one tiny next step.*

---

## 2) Target user

**Maya, 34. Diagnosed with ADHD at 31** — one of the ~half of the 15.5M US adults with a current ADHD diagnosis who were diagnosed in adulthood ([CDC MMWR 73:890-895](https://www.cdc.gov/mmwr/volumes/73/wr/mm7340a1.htm)). Knowledge worker by day; by night and weekend she is the owner of a project graveyard: a novel at 60%, a half-built Etsy shop, a room that's been two colors for a year, a job hunt she keeps "restarting." Her pattern is the documented one: "the first 20% offers novelty, the last 20% offers completion's reward, but the middle offers neither" ([Tiimo](https://www.tiimoapp.com/resource-hub/finishing-what-you-start-adhd)), and the last few years look like the community's "hobby hopping" blur — gardening, Spanish, painting, 3D modeling, each started with intensity and abandoned ([Simply Psychology](https://www.simplypsychology.org/adhd-hobby-jumping.html)).

She has tried Notion (abandoned — "an organizational decision at the moment of every save… the system gets silently abandoned despite an enthusiastic setup" — [MindStash](https://www.mindstash.app/blogs/why-your-adhd-brain-hates-notion-and-what-actually-works-instead)), Apple Notes (thousands of orphans — cf. the writer who decluttered 4,000 of them, [Medium](https://medium.com/macoclock/how-i-decluttered-4-000-apple-notes-in-15-minutes-a-week-7417819ba159)), and a planner or two. She can't afford a coach ($170–225/session, rarely insured — [Coaching Executive Function](https://www.coachingexecutivefunction.com/post/how-much-does-adhd-coaching-cost)). What she wants isn't another planner. She wants to stop losing the plot of her own life.

**[INFERENCE]** We deliberately target the *multi-project adult* (side projects, life admin arcs, creative work) rather than the student or the pure-workplace user: this persona has the most simultaneous open threads, the longest gaps between touches, and therefore the highest re-entry pain per week.

---

## 3) Winning problem + evidence

**Project re-entry friction — returning to anything after time away feels like starting from zero — plus its upstream cause, thought/context loss.** Our verification report rates this the single best wedge: "confirmed as distinct, severe, and underserved… Existing apps attack capture/organize, not context-restoration" (verification.md).

- "Projects often die because you lose context — you open the document and can't remember where you were or what you were thinking, the shape of the project that was so clear has dissolved, and the re-entry cost feels enormous." — [Fabric](https://fabric.so/blog/how-to-finish-projects-when-your-brain-keeps-starting-new-ones)
- "When ADHD working memory is overloaded, restarting after an interruption may feel like beginning the whole task again. That friction makes restarting harder than starting something new." — [ADHD Philadelphia](https://www.adhdphiladelphia.com/blog/why-adults-with-adhd-lose-momentum-so-easily-after-interruptions)
- Real-world cost: a spouse describing 2.5 years without a kitchen amid endless unfinished projects — [ADHDandMarriage forums](https://www.adhdmarriage.com/content/2-12-years-w-no-kitchen-endless-unfinished-projects-homehelp-me-understand)
- The upstream failure is capture: "when it vanishes, it's like being betrayed by your own mind" ([InFocus First](https://infocusfirst.com/adhd-forgetfulness/)); working-memory mechanism confirmed independent of vendor content ([CHADD](https://chadd.org/adhd-weekly/adhd-can-trip-up-memories/)).
- Every existing mitigation (breadcrumb notes, work journals) "requires remembering to leave notes at the exact moment focus breaks — precisely what ADHD prevents" (task-paralysis-findings.json; corroborated by [ADHD Homestead](https://adhdhomestead.net/adhd-project-engineering-part-2/), [first-person dev account](https://dev.to/terrizoaguimor/i-have-adhd-and-i-keep-losing-context-so-i-taught-my-ai-to-remember-for-me-afl)).

**Honesty clause (required by verification.md):** re-entry pain is qualitatively convergent across independent source types but its frequency/severity has never been quantitatively measured. Section 9 is our plan to measure it in month one.

Why this problem wins commercially: capture and breakdown are saturated (AudioPen, Voicenotes, Goblin Tools, Saner.AI); planners are saturated (Tiimo, Structured, Motion). "Only the resurface/re-entry layer is thin" (verification.md) — and "memory is unclaimed. No product remembers where you were and briefs you back in after absence" (competitor-teardowns.md).

---

## 4) Core loop — why does she open Ember *every day*?

The trap in a memory-first product is that re-entry is episodic. Our answer: **the daily verb is depositing; the payoff verb is returning.** Like a bank, you visit daily to deposit, and the account is why you stay. Every layer of the loop is built on the cross-cutting truths: zero-decision capture (truth: filing decisions kill capture), point-of-performance externalization (Barkley), instant reward (delay-discounting), designed-for-absence (truth #2).

**The daily loop, step by step:**

1. **Catch (5–25×/day, 2 seconds each).** A thought arrives — in a meeting, in the shower doorway, at 1am. Maya hits the lockscreen widget / share sheet / watch complication, speaks or types, and closes her phone. No folder, no tag, no title, no app navigation. *Why she does it daily:* this is the lowest-friction externalization of the exact anxiety her brain runs on — "your brain races because it's afraid of forgetting" ([Built for ADHD](https://www.builtforadhd.com/blog/adhd-brain-dump-method-clear-47-mental-tabs/)). The reward is immediate and visible: within seconds the capture appears *already filed* into the right project thread ("Caught → filed to *Etsy shop*"), collapsing the action→reward delay ([JAD delay-discounting meta-analysis](https://journals.sagepub.com/doi/10.1177/1087054718772138)). Capture is useful on day one with zero corpus — no cold-start.
2. **The Doorway (1×/day, 90 seconds).** One warm morning card — not a task list, not a schedule: *"Three threads are warm today. Your tax thread has a deadline getting close — the next pebble is 'find the 1099 email' (about 5 min). Also, you had a good idea about the shop logo on Tuesday — want to look?"* One suggested tiny step, one resurfaced thought, nothing else. It replaces the anxious mental scan of "what am I forgetting?" — the single question ADHD working memory can't answer ([ATTN Center](https://attncenter.nyc/understanding-working-memory-in-adhd-how-to-remember-not-to-forget/)). Novelty by design: the Doorway's content, framing, and one-liner vary daily to fight habituation ([Brain, novelty-processing](https://academic.oup.com/brain/article/141/5/1545/4934119); reminder-habituation evidence: [Sprout](https://www.sproutapp.tech/blog/adhd-reminder-app)).
3. **Touch a thread (whenever she acts).** If she opens a project, Ember shows a **Warm Start** briefing scaled to time-away (a one-liner after a day; a full brief after weeks). Doing the tiny step takes minutes; marking it done feeds tomorrow's Doorway.
4. **Auto-breadcrumb on exit (0 effort).** When she stops — mid-task, as ADHD focus breaks do — Ember writes the breadcrumb *for* her from what she captured, checked, and did: "You stopped after drafting the email to the accountant; you were unsure about the deduction question." This is the mechanism that fixes the fatal flaw of every manual system: the breadcrumb no longer depends on the impaired function.
5. **Night catch (many nights).** Insomnia affects 66.8% of adults with ADHD vs 28.8% of controls ([Brevik et al., peer-reviewed](https://pubmed.ncbi.nlm.nih.gov/28547881/)). Night mode is a black, silent, single-field screen — no feed, no red badges, no dopamine — thoughts route to tomorrow's Doorway. It closes the loop: "dumping everything gives it permission to shut down" ([Built for ADHD](https://www.builtforadhd.com/blog/adhd-brain-dump-method-clear-47-mental-tabs/)).

**Designed for absence:** if Maya vanishes for three weeks (she will — median 30-day retention for mental-health apps is ≈3.3%, [Baumel et al. 2019, JMIR](https://www.jmir.org/2019/9/e14567/)), Ember gets *more* valuable, not more shaming. She returns to zero overdue badges and one message: "Welcome back. Nothing is lost. Here's what's still warm." Absence is our demo, not our churn event.

---

## 5) Core modules

**M1 — Catch: zero-decision capture.** Voice or text from lockscreen widget, share sheet, watch, web clipper, and night mode. The user makes exactly zero organizational decisions; a fast model classifies each capture into an existing thread, proposes a new thread, or parks it in a visible "Loose sparks" tray (never a hidden inbox — "out of sight, out of mind is a neurological reality," [Medium/Brunell](https://raymond-brunell.medium.com/i-deleted-47-productivity-apps-in-30-days-heres-what-actually-worked-for-my-adhd-brain-52c292c6ba6b)). Misfiles are corrected with one drag, and corrections teach the classifier. Painless import of the wreckage — Apple Notes, Notion exports, voice memos — so past graveyards become memory, not guilt. *(Founder mandate 1: organize thoughts.)*

**M2 — Threads: the living project memory.** Everything in Ember lives on a thread — a project, a person, a life-admin arc, an idea cluster. A thread accrues captures, next steps, decisions, links, and auto-breadcrumbs into a chronological "story of this project." The Shelf is the browse surface: warm threads up front, paused threads resting on an explicit **amnesty rack** (paused ≠ failed; no red, no overdue math — streaks and overdue walls are evidenced harms: [Kabit](https://kabitapp.com/blog/habit-tracker-adhd), [medRxiv streak-backfire preprint](https://www.medrxiv.org/content/10.1101/2024.12.26.24319676.full.pdf)). Hybrid search (semantic + keyword + "around the time I was into climbing") answers "I know I wrote it down somewhere" ([Passionate Writer Coaching](https://passionatewritercoaching.com/best-free-note-taking-apps/)). *(Founder mandate 3: view, pick up, and find thoughts and projects.)*

**M3 — Warm Start: the re-entry briefing (crown jewel).** Open anything after time away and Ember briefs you in, scaled to the gap and written warmly in second person: where you were, what you were thinking (verbatim quotes from your own captures — your past self talking to you), why you cared, what changed while you were gone, and **one tiny next step sized to restart momentum, not to finish**. Every briefing ends with four buttons: *Do the tiny step · Snooze it · Shrink the project · Retire it with honor.* Retirement writes a closing note ("You built the hard part. It taught you resin casting.") — converting the project graveyard from shame into history. This directly serves the middle-60% dead zone where projects stall ([Tiimo](https://www.tiimoapp.com/resource-hub/finishing-what-you-start-adhd)).

**M4 — Year Arc: breakdown that survives months.** Any big thing — taxes, a move, a job hunt, a novel — becomes an arc of milestones across the year, decomposed by AI *with the thread's context* (deadlines, what's already done, her stated energy patterns), not the generic one-shot lists that made Goblin Tools "a planning tool, not an execution tool" ([Thawly](https://thawly.ai/reviews/goblin-tools)). Ember shows only the **next pebble** — one small step — never the wall of steps; the arc silently re-plans when reality changes ("all the steps merge into one big, intimidating task" — [ADDitude](https://www.additudemag.com/where-do-i-start-adhd-organization/)). Deadline-bearing arcs surface in the Doorway with gentle backwards-planned lead time. *(Founder mandate 2: break down big tasks workable throughout the year.)*

**M5 — The Doorway: the daily anchor.** Described in §4. Strictly bounded: one card, ≤3 items, one tiny step, done in 90 seconds — a ritual small enough that it cannot become "another thing to fail at" (Sunsama's documented failure mode — [rivva](https://blog.rivva.app/p/best-sunsama-alternatives-for-adhders)). Notifications are capped, varied in wording, and self-silencing when ignored (anti-habituation, [My Patient Advice](https://mypatientadvice.co.uk/knowledge-base/why-do-adhd-brains-still-ignore-phone-alarms/)).

---

## 6) Why AI is necessary — and what deliberately is NOT AI

**Necessary (the product is impossible without it):**
- **Zero-decision filing** requires semantic classification of messy fragments into evolving threads. Rules and folders reintroduce the filing decision that kills capture.
- **Warm Start briefings** require *synthesis*: compressing weeks of fragments, actions, and breadcrumbs into a short, warm, second-person narrative with a well-chosen tiny step. This is summarization + tone + judgment — the core LLM competency, and the thing a human coach does for $170+/hr.
- **Context-aware breakdown & re-planning** (Year Arc) must know the thread's history — otherwise it's another Goblin list that "starts every session fresh" ([FocusHack](https://www.focushack.io/reviews/goblin-tools-adhd-review/)).
- **Natural-language recall** ("that idea about the logo, sometime in spring").

**Deliberately NOT AI:**
- **Reminders, deadline math, notification scheduling** — deterministic and inspectable. No AI deciding when to interrupt you.
- **No auto-scheduling of your calendar.** Motion proves that maximal automation of a neurotypical planning model produces oppressive schedules ([Saner.AI roundup](https://www.saner.ai/blogs/motion-reviews)). Ember never packs your day.
- **No AI companion/chatbot as the primary surface.** Chat is a blank-page decision; Ember's surfaces are buttons and cards.
- **Progress display, amnesty mechanics, billing** — plain code, plain honesty.
- **Capture itself** — recording a thought must work offline, instantly, with AI applied after the fact; transcription on-device where the platform allows. **[INFERENCE]** on-device Whisper-class transcription is feasible on modern phones.

---

## 7) Differentiation — and why we won't die like Mem/Napkin

| Competitor | What they own | What Ember does that they structurally can't |
|---|---|---|
| **Tiimo** ($12/mo) | Today's visual schedule | Tiimo "has no project memory, no thought capture, no execution support… shows a *plan*, not a *path into motion*" (competitor-teardowns.md). Ember owns the weeks-and-months axis Tiimo never touches; we can coexist on a user's phone and still win the subscription. |
| **Goblin Tools** (free/$3.49) | One-shot breakdown | "Generates a beautiful list of steps and then stops at the plan" ([Thawly](https://thawly.ai/reviews/goblin-tools)); "no persistence… each session starts fresh" ([FocusHack](https://www.focushack.io/reviews/goblin-tools-adhd-review/)). Ember's breakdown is a *living arc* attached to memory. Goblin validated demand and its free tier can't fund memory infrastructure. |
| **Motion** ($19+/mo) | AI auto-scheduling | Optimizes for "fitting everything in" → oppressive for ADHD ([Saner.AI](https://www.saner.ai/blogs/motion-reviews)); heavy onboarding; unpredictable AI-credit pricing ([Morgen](https://www.morgen.so/blog-posts/motion-pricing)). Ember is capacity-forgiving by design and prices flat and honest. |
| **Saner.AI** ($8–16/mo) | "Jarvis for ADHD" notes+chat | Closest in spirit but chat-first, single-slice, ~5 people/~$120K pre-seed (verification.md) — and it stops at capture/organize without closing the loop to re-entry at the moment of return. Our wedge starts exactly where they stop. |

**The Mem/Napkin steelman — the graveyard we must not join.** Mem raised $23.5M and is written up as a failure case; Napkin shut down its desktop app (verification.md). Their shared thesis — *passively resurface old notes and users will find serendipitous value* — failed for four identifiable reasons, and Ember is designed against each:

1. **They resurfaced *notes*; notes carry no stakes.** A random old note has no "so what." Ember resurfaces **projects and commitments with a next action attached** — the verification report's explicit prescription ("resurface commitments and projects with next actions… not a serendipity lottery").
2. **Push-based serendipity vs pull-based point of performance.** Mem/Napkin interrupted you with the past at arbitrary moments. Ember's crown-jewel moment is **pull-based**: the briefing fires when *you* return to a thread — exactly Barkley's point of performance ([Barkley factsheet](https://www.russellbarkley.org/factsheets/ADHD_EF_and_SR.pdf)). Push exists only inside the bounded daily Doorway, capped at one resurfaced item with an action.
3. **Cold start: their value required a large corpus.** Ember is useful in the first hour with zero corpus: capture relief is immediate, and Year Arc breakdown works on the first project. Memory value compounds *on top of* a tool that already earns its place.
4. **No daily job, no emotional job.** Note tools were emotionally neutral utilities with weekly-at-best jobs. Ember has a many-times-daily job (Catch), a bounded daily ritual (Doorway), and an explicitly emotional job — shame-safe return — for an audience with a documented shame spiral ("the symptoms create shame, while the shame makes the symptoms worse" — [Heal & Thrive](https://heal-thrive.com/adhd-and-shame-spirals-how-one-bad-day-becomes-a-week-of-avoidance/)). Warmth is retention: Finch proved emotional safety retains this audience (competitor-teardowns.md).

**Trust wedge:** flat price, two-tap cancellation, and auto-pause of billing after 45 days of inactivity with a "welcome back, we paused you" note — in a category defined by cancellation complaints ([Tiimo Trustpilot](https://www.trustpilot.com/review/tiimo.dk), [Inflow — charged despite cancellation](https://www.choosingtherapy.com/inflow-adhd-app-review/), [Future ADHD scammer callout](https://futureadhd.com/articles/adhd-product-scammer-callout/)). For ADHD users who pay a literal "forgotten-subscription tax," auto-pause is a feature no incumbent will copy quickly because it costs them their dark-pattern revenue. **[INFERENCE]** It also converts our biggest risk (absence) into a loyalty event.

---

## 8) Cost & pricing sketch

Model tiers (current list prices): **Haiku 4.5** $1.00/$5.00 per MTok in/out; **Sonnet 4.6** $3.00/$15.00. Prompt caching (stable system prompts + thread digests) makes cached input ~0.1×; nightly jobs use the Batch API at 50% off. Rough math for a **high-usage** user (P90, not average):

| Workload | Model | Volume/mo | Tokens (in/out per call) | Cost/mo |
|---|---|---|---|---|
| Capture triage & filing | Haiku 4.5 | 750 (25/day) | 1,000 / 150 (system prompt cached) | **$1.31** |
| Daily Doorway | Sonnet 4.6 | 30 | 3,000 / 300 | **$0.41** |
| Warm Start briefings | Sonnet 4.6 | 20 | 6,000 / 500 | **$0.51** |
| Year Arc breakdown / re-planning | Sonnet 4.6 | 15 | 3,000 / 800 | **$0.32** |
| Nightly thread-digest maintenance | Haiku 4.5, Batch (50%) | 30 | 4,000 / 400 | **$0.09** |
| Voice transcription (server fallback; on-device default) | ASR | ~150 min | — | **~$0.35** |
| Embeddings + search | small embed model | ~1,500 items | — | **~$0.05** |
| **Total, high-usage** | | | | **≈ $3.04** |

Headroom to the $5.99 ceiling ≈ 2×, absorbing retries, longer briefings, and heavy importers; median users will land near ~$1. Digest-first architecture keeps context bounded: briefings read a maintained per-thread digest, never the raw history. Killswitch: if a user's spend trends past $5, Doorway/digest quietly drop to Haiku.

**Pricing:** **$8.99/mo or $79/yr**, 21-day full trial, no card up front. At $8.99 with ~$3 worst-case COGS-AI, gross margin stays >60% even before median-user blending — profitable under the founder's constraint, and priced under Tiimo monthly, Inflow, and Motion with far more AI inside (price umbrella per competitor-teardowns.md).

---

## 9) Riskiest assumption + one-month validation

**The assumption:** *ADHD users will capture into Ember frequently enough (despite Apple-Notes muscle memory and app-abandonment patterns) that briefings are rich when they return — and re-entry pain, which is qualitatively convergent but unmeasured, is frequent and severe enough to anchor a paid subscription.* If either half fails, the memory flywheel never spins.

**Month-one validation plan:**
- **Week 1–2 — quantify the pain (fixes the evidence gap flagged in verification.md).** Landing-page smoke test, two variants A/B: re-entry-led ("come back to any project like you never left") vs capture-led ("never lose a thought"). Spend ~$500 on ADHD-community-adjacent traffic; measure email conversion per variant. In parallel, 20 structured interviews recruited from ADHD subreddits/Discords with severity instruments: count of stalled projects, date of last re-entry attempt, minutes lost, what they'd pay. **Threshold:** ≥25% name re-entry/lost-context pain unprompted in their top two; re-entry variant converts within 25% of capture variant or better.
- **Week 2–4 — Wizard-of-Oz concierge.** 10–15 users capture via a Telegram/WhatsApp bot; an operator + Claude produce the nightly filing, daily Doorway, and on-demand Warm Start briefings by hand. Measures: capture frequency curve, D14 capture retention, briefing → action rate (did the tiny step happen within 48h?), qualitative "betrayed-by-my-own-mind" relief signal. **Threshold:** ≥40% still capturing at day 14 (vs the ≈3.3% 30-day category baseline, [Baumel 2019](https://www.jmir.org/2019/9/e14567/)); ≥50% of briefings rated "I could restart from this alone."
- **Week 4 — willingness to pay.** Offer founding pre-order ($20 credited year-one) to all participants. **Threshold:** ≥3 of 15 pay. Below thresholds → pivot the wedge toward the Doorway-as-anchor or narrow to one high-pain arc (taxes/job hunt) before writing code.

---

## 10) Why we beat the other teams

**vs. Execution-first (paralysis/"wall of awful" thesis).** The pain is real (severity 5/5) but the verification report explicitly flags it as *not fully underserved* — Focusmate, Flow Club, Dubbii, and every body-doubling product already attack initiation, and the "tools stop at the plan" narrative circulates mainly on competitor-SEO blogs, meaning that wedge is contested (verification.md). An in-the-moment activation product also owns no data: nothing compounds between sessions, so it's a feature any incumbent (or Ember) can bolt on — and we do: every Warm Start ends in one tiny, pre-chosen step delivered at the point of performance, which *is* execution support, minus the freeze-detection science fiction. Meanwhile their product has nothing to say about the months-long arc where projects actually die.

**vs. Daily-anchor-first (planner/schedule thesis).** That lane is the most saturated in the category — Tiimo just won Apple's App of the Year in it (competitor-teardowns.md) — and it fights the two deadliest truths in the research: "the list was never the problem" and "any demanded consistency will not be supplied" (problem-hunt.md). A daily-anchor product's value is *destroyed* by absence: miss a week and the anchor is a shame wall (Sunsama's "another thing to fail at"). Ember inverts that: our value is *created* by absence, because absence is what makes memory and re-entry precious. We still get a daily anchor — the 90-second Doorway — but as a bounded byproduct of memory, not as the product itself, so we never bet retention on the consistency ADHD cannot supply.

**The structural argument:** Ember is the only thesis with a compounding asset. Every capture, breadcrumb, and correction deepens a personal project memory that (a) makes briefings better, (b) raises switching costs far beyond any planner or activation tool, and (c) is the substrate on which the *other two theses' features can be built later* — execution nudges and daily planning both work better sitting on top of memory than standing alone. The reverse is not true. Judges should ask each team one question: *what happens to your product when the user disappears for three weeks?* Team A's freeze-helper goes unused. Team C's planner becomes a guilt monument. Ember writes the welcome-back briefing. That's the whole market.
