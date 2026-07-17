# Team C Thesis — Daily-Anchor-First

## 1) Product name + one-line pitch

**Anchor** — *The AI daily anchor that plans a day you can actually do, and quietly forgives the days you can't.*

Anchor is not a planner you operate. It is a rhythm that operates for you: a 60-second morning brief built around your real capacity, felt-time support through the day, and a zero-guilt evening close that rolls everything forward — with your thoughts and projects living inside that rhythm, so there is always a moment for them to resurface into.

## 2) Target user

**Maya, 32.** Program coordinator at a mid-size nonprofit, hybrid schedule, diagnosed with ADHD at 29 — one of the ~half of the 15.5M US adults with a current ADHD diagnosis who were diagnosed in adulthood ([CDC MMWR 73:890-895](https://www.cdc.gov/mmwr/volumes/73/wr/mm7340a1.htm)). She is the "You have 10 unused planners, and you buy one more because it's shiny and new" reader ([ADDitude reader submission](https://www.additudemag.com/adhd-memes-jokes-relatable-funny/)): she has installed Tiimo, Structured, Todoist, and Notion, set each one up in a hyperfocused evening, and abandoned each within three weeks — not because they were bad, but because they demanded the daily consistency her brain cannot supply, then greeted her return with a wall of red overdue tasks. Her days are eaten by "waiting mode" before 2pm meetings ([Healthline](https://www.healthline.com/health/adhd/adhd-waiting-mode)), her side projects stall at 80% ([Tiimo resource hub](https://www.tiimoapp.com/resource-hub/finishing-what-you-start-adhd)), and one-third of adults like her report time-management problems as the single greatest source of stress in their lives ([ADDitude survey of 1,859 adults](https://www.additudemag.com/punctuality-time-blindness-adhd-apps-tips/)). She will pay ~$9/month for something that works *with* inconsistent engagement — she already pays far more in ADHD tax.

She is explicitly **not** a $170–225/hr coaching client ([Coaching Executive Function](https://www.coachingexecutivefunction.com/post/how-much-does-adhd-coaching-cost) — confirmed band, verification.md) and not a Motion-style power-planner user.

## 3) Winning problem + evidence

**The problem is not that ADHD adults lack a planner. It's that no planner survives contact with an ADHD life.** Every incumbent assumes the user will show up daily, plan realistically, and return unashamed after lapses — precisely the three things the condition impairs. The result is serial abandonment:

- Median 30-day retention for mental-health apps is **3.3%** ([Baumel et al. 2019, JMIR e14567](https://www.jmir.org/2019/9/e14567/) — corrected citation per verification.md). Abandonment is the default outcome of this category.
- "Any demanded consistency will not be supplied… design for inconsistent engagement" ([Tiimo resource hub](https://www.tiimoapp.com/resource-hub/why-productivity-systems-fail-adhd); cross-cutting truth #2, problem-hunt.md).
- Sunsama's defining crack: "the 15-20 minute planning window requires its own executive function to initiate… the ritual can become 'another thing to fail at'" ([rivva](https://blog.rivva.app/p/best-sunsama-alternatives-for-adhders), [Business Dive](https://thebusinessdive.com/sunsama-review)).
- Motion "optimizes for fitting everything in," producing oppressive capacity-blind schedules ([Saner.AI roundup](https://www.saner.ai/blogs/motion-reviews)).
- Streaks are "the single worst design choice for ADHD users" ([Kabit](https://kabitapp.com/blog/habit-tracker-adhd)); the streak-backfire mechanism is independently supported ([medRxiv preprint](https://www.medrxiv.org/content/10.1101/2024.12.26.24319676.full.pdf), verification.md ✅).
- Reminders habituate: "the notification sound that once jolted you into action now barely registers" ([My Patient Advice](https://mypatientadvice.co.uk/knowledge-base/why-do-adhd-brains-still-ignore-phone-alarms/)); no mainstream product implements anti-habituation as a core mechanic (time-life-admin-findings.json, P6).
- Time is displayed, not felt: calendars show clock time; the unserved slices are backwards-planning "start now to make Y" and waiting-mode-sized suggestions ([Sagebrush Counseling](https://www.sagebrushcounseling.com/blog/what-is-adhd-waiting-mode); problem-hunt P4).
- The stakes are financial, not cosmetic: ADHD adults' credit default rates grow exponentially in middle age ([Science Advances aba1551](https://www.science.org/doi/10.1126/sciadv.aba1551)), and they are 60–90% more likely to miss GP appointments ([University of Bath](https://www.bath.ac.uk/announcements/new-study-reveals-high-rates-of-missed-gp-appointments-among-patients-with-adhd/) — ✅ confirmed).

**Winning framing:** the daily plan is the only surface with *native daily frequency* — and therefore the only surface from which capture, breakdown, execution nudges, and project re-entry can actually be delivered at the point of performance (Barkley's externalization principle, [factsheet](https://www.russellbarkley.org/factsheets/ADHD_EF_and_SR.pdf)). Whoever owns a daily anchor that survives chaos owns the delivery channel for everything else.

## 4) Core loop

1. **Capture anytime (zero decisions).** Maya dumps thoughts by text or voice into one inbox — no folder, no tags, no filing choice ("Notion requires an organizational decision at the moment of every save… It's a design-fit problem, not a willpower problem" — [MindStash](https://www.mindstash.app/blogs/why-your-adhd-brain-hates-notion-and-what-actually-works-instead)). A cheap model files each item to a project or the day silently.
2. **Morning brief (60 seconds, consumed — not authored).** Anchor reads her calendar, open tasks, project states, and recent completion history and *proposes* a day: 3–5 realistic items, backwards-planned around fixed events ("leave by 1:15 to make the 2:00"), with one tap to accept, swap, or shrink. A **capacity dial** (good day / medium / low-spoon) resizes the whole day in one tap — a low-spoon day is 1 anchor task + self-maintenance, presented as a *complete plan*, not a failure state.
3. **Day companion (point of performance).** Leave-by countdowns; waiting-mode gaps filled with right-sized suggestions ("You have 47 min before the 2:00 — enough for the pharmacy call, not the report"); reminders whose wording, tone, and timing vary every time so they never fade into wallpaper (novelty fights habituation — [Brain](https://academic.oup.com/brain/article/141/5/1545/4934119)).
4. **Zero-guilt evening close (works with zero input).** At a chosen hour, Anchor closes the day itself: done items celebrated, undone items rolled forward silently — no red, no overdue count, no streak to break (Tiimo's no-shame rollover proved this pattern; Finch proved warmth retains). If she touched a project, Anchor writes a breadcrumb note for future-her.
5. **Loop resumes — including after absence.** Tomorrow's brief starts from today's true state. If Maya vanishes for 12 days, there is no punishment wall: she gets a single "welcome back" brief — what mattered while she was gone, what quietly expired, one suggested first step. The system assumed absence; returning costs nothing.

Capture and project breakdown live *inside* this loop: every project's next action is a candidate for tomorrow's brief, and every captured thought lands somewhere findable — so the daily rhythm is also the resurfacing engine.

## 5) Core modules

**A. Morning Brief + Capacity Dial.** The heart of the product. Anchor generates a proposed day from calendar events, rolled-forward tasks, project next-actions, and the user's actual historical throughput — deliberately planning *under* capacity, the inverse of Motion's fit-everything optimizer ([Saner.AI](https://www.saner.ai/blogs/motion-reviews)). The capacity dial makes "low-spoon mode" a first-class plan rather than a failure of the real one, applying implementation-intention structure (if-then, concrete first steps; d = .65 on goal attainment, [Gollwitzer & Sheeran 2006](https://www.socmot.uni-konstanz.de/publications/implementation-intentions-and-goal-achievement-meta-analysis-effects-and-processes) — general-population evidence, per verification.md). Crucially, the brief is *consumed, not created*: Sunsama's fatal 15-minute ritual becomes a 60-second approval.

**B. The Inbox That Files Itself (founder mandate: organize thoughts).** One capture surface — text, voice, share-sheet — with zero filing decisions at save time. An LLM classifies each item to a project, the day, or a someday shelf, and everything remains findable by natural-language search ("that thing about mom's birthday gift"). This directly attacks capture-abandonment ("I know I wrote it down somewhere… But is it in my Apple Notes, my Kindle highlights, Scrivener…?" — [Passionate Writer Coaching](https://passionatewritercoaching.com/best-free-note-taking-apps/)) and the night dimension: a bedside brain-dump mode for the 66.8% of ADHD adults with insomnia vs 28.8% of controls ([Brevik et al., peer-reviewed](https://pubmed.ncbi.nlm.nih.gov/28547881/)). We deliberately do *not* compete as a notes app — capture exists to feed the brief.

**C. Project Shelf + Year Runway (founder mandates: break down big tasks across the year; view/pick up/find projects).** Every big goal ("redo the kitchen," "job hunt," "novel draft") gets an AI-generated breakdown into seasons → weeks → next actions, workable across a year. Projects live on a visual Shelf where each card shows its state, its breadcrumb ("where you were, what you were thinking"), and its single next action. The brief drip-feeds one project action per day so the middle 60% of a project — where "the first 20% offers novelty, the last 20% offers completion's reward, but the middle offers neither" ([Tiimo](https://www.tiimoapp.com/resource-hub/finishing-what-you-start-adhd)) — gets carried by the rhythm instead of by motivation. Picking up a dormant project triggers a re-entry card: 30-second context restoration instead of the "re-entry cost [that] feels enormous" ([Fabric](https://fabric.so/blog/how-to-finish-projects-when-your-brain-keeps-starting-new-ones)).

**D. Felt-Time Engine.** Translates clock time into felt time at the point of performance: backwards-planned "start now to make it" alerts with prep steps inferred (shower + dress + transit, not just "meeting at 2"); waiting-mode gap-filling with tasks sized to the gap and safe to drop; and **anti-habituation reminders** — copy, framing, and escalation generated fresh so the brain never files them as background noise. This is the P4 + P6 bundle no incumbent ships (problem-hunt.md), and it addresses a population where missed appointments are measured at scale ([PLOS Mental Health, 824,374 patients](https://journals.plos.org/mentalhealth/article?id=10.1371%2Fjournal.pmen.0000045)).

**E. Zero-Guilt Close + Absence Protocol.** The close requires no input to function, rolls everything forward without visual debt, and writes project breadcrumbs automatically. The Absence Protocol is the retention moat: the system is *architected for the lapse* — notifications taper instead of nagging, nothing accumulates into a shame wall, and the welcome-back brief compresses any gap into one screen. Shame-safe design is an evidence-backed requirement, not a flourish (verification.md ✅: streak mechanics backfire; Todoist's "task graveyard" shame loop — [Thawly](https://thawly.ai/reviews/todoist-for-adhd)).

## 6) Why AI is necessary — and what deliberately is NOT AI

**Necessary (couldn't ship pre-LLM):**
- **Capacity-aware triage.** Choosing *which 3 of 47 items* fit today, given calendar, energy signal, deadlines, and what this user historically completes on a Tuesday, is judgment, not rules. Rule-based planners either show everything (overwhelm) or demand the user choose (the impaired function).
- **Zero-decision capture.** Parsing "ugh call the dentist before friday also idea: pitch anna the workshop thing" into two correctly-filed items with dates requires language understanding. This is the exact failure that kills Notion for ADHD users.
- **Anti-habituation at scale.** Infinite non-repeating reminder copy, tone-matched and consequence-calibrated, is impossible with template systems — and habituation is why static reminders die within a week ([Sprout](https://www.sproutapp.tech/blog/adhd-reminder-app)).
- **Breakdown + re-entry synthesis.** Year-scale project decomposition and "here's where you were" briefs are generative tasks.
- **Prep-step inference for backwards planning** ("to make the 2:00 you need to leave by 1:15, which means lunch by 12:30").

**Deliberately NOT AI:** timers and countdowns; notification delivery and scheduling; calendar sync; the data model (tasks/projects/notes are plain structured records the user owns and can export); completion tracking; the capacity dial itself (user-controlled, never inferred and imposed); and there is **no AI companion chatbot and no AI therapy** — Anchor is an executive-function prosthetic, not a clinical product, and the reading-heavy CBT lane is both served and resented ([Choosing Therapy on Inflow](https://www.choosingtherapy.com/inflow-adhd-app-review/)). Also no streaks, no gamification, by design.

## 7) Differentiation vs Tiimo / Structured / Motion / Sunsama — why we win the most crowded lane

We accept the verification finding head-on: planners are the most saturated lane — Tiimo won Apple's 2025 iPhone App of the Year ([Daring Fireball](https://daringfireball.net/2025/12/2025_app_store_award_winners)), Structured has ~1.5M users. **Saturation here proves two things: the demand surface is enormous, and the failure mode is known and unclaimed.** Every incumbent monetizes the install; none has solved the 3.3% 30-day cliff ([JMIR e14567](https://www.jmir.org/2019/9/e14567/)). The lane is crowded at acquisition and empty at retention.

The difference in kind, not degree: **incumbents sell software for making plans; Anchor sells the plan itself.** In every existing planner, consistency must live in the user — you build the timeline (Structured), drag the blocks (Tiimo), perform the ritual (Sunsama), or feed the optimizer (Motion). In Anchor, consistency lives in the system: the artifact is *generated for you each morning from persistent memory* and regenerates no matter how you behaved yesterday. That inversion is only possible AI-native, and it changes the failure mode — when the user lapses, an incumbent's value drops to zero (empty timeline, red backlog); Anchor's value is *highest at the lapse* (the welcome-back brief).

- **vs Tiimo:** Tiimo is a beautiful canvas that owns "today's schedule" but "shows a *plan*, not a *path into motion*" (competitor-teardowns.md) — no project memory, no capture, reported reminder failures ([JustUseApp](https://justuseapp.com/en/app/1480220328/tiimo-visual-daily-planner/reviews)) and "almost impossible" cancellation complaints ([Trustpilot](https://www.trustpilot.com/review/tiimo.dk)). We generate what Tiimo makes you assemble, and we bill honestly (one-tap cancel) as a trust wedge in a category defined by billing complaints (verification.md ✅).
- **vs Structured:** a manual timeline with 1.5M installs is 1.5M people who proved demand and will churn at the first chaotic week. We are their second chance, priced below their frustration.
- **vs Motion ($19+/mo):** Motion automates the neurotypical planning model harder — packs the calendar full. We are its inverse: capacity-first, under-plan by design, at half the price.
- **vs Sunsama ($16-20/mo):** Sunsama's guided ritual *is* the EF tax. Our brief costs 60 seconds and zero initiation — the ritual is performed by the product.
- **vs the AI-native wave (Saner.AI, rivva, Fomi):** single-slice, early, low-trust (competitor-teardowns.md). None owns the full daily container; "the lifecycle bundle — capture → breakdown → execution → re-entry — is claimed by no one."

## 8) Cost & pricing sketch

Model tiers (current API pricing): **Haiku 4.5** $1/$5 per MTok in/out; **Sonnet 4.6** $3/$15. Prompt-cache reads ~0.1×; Batch API −50%. Architecture rule: Haiku for high-frequency/low-judgment calls, Sonnet only for the two daily judgment moments, batch + caching for everything precomputable.

High-usage user (30 days, generous assumptions):

| Workload | Model | Calls/mo | Tokens (in/out per call) | Cost/mo |
|---|---|---|---|---|
| Morning brief (plan synthesis) | Sonnet 4.6 | 30 | 6K / 800 (≈3K of input cache-read) | **$0.78** |
| Evening close + breadcrumbs + rollover | Sonnet 4.6 | 30 | 4K / 600 | $0.63 |
| Capture classification/filing | Haiku 4.5 | 450 (15/day) | 1.2K / 120 | $0.81 |
| Anti-habituation reminder copy (nightly batch, −50%) | Haiku 4.5 | 30 batches | 2K / 1.5K per batch | $0.14 |
| Project breakdowns / re-plans | Sonnet 4.6 | 12 | 3K / 1.5K | $0.38 |
| Re-entry briefs + shelf search | Sonnet 4.6 / Haiku | ~40 | 4K / 500 avg | $0.45 |
| Waiting-mode / felt-time suggestions | Haiku 4.5 | 150 | 1.5K / 200 | $0.38 |
| **Total (high usage)** | | | | **≈ $3.57** |

Headroom to the $5.99 ceiling ≈ 40%, absorbing retries, longer contexts, and heavy months; median users (briefs + light capture) land near **$1.50–2.00**. Cost guards: brief regeneration capped at 3/day, stable system prompts under prompt caching, overnight precomputation of reminder copy and draft briefs via Batch API. [INFERENCE: token sizes are engineering estimates; validated in week one of the concierge test by metering real transcripts.]

**Pricing: $8.99/mo or $59/yr. Honest billing as a feature:** one-tap in-app cancellation, no charge-after-cancel, pause-your-subscription for lapses. This is evidence-backed positioning, not virtue: the community loudly resents billing dark patterns ([Future ADHD](https://futureadhd.com/articles/adhd-product-scammer-callout/); Inflow charged-after-cancellation complaints — [Choosing Therapy](https://www.choosingtherapy.com/inflow-adhd-app-review/); Tiimo cancellation complaints — [Trustpilot](https://www.trustpilot.com/review/tiimo.dk)). Gross margin at high usage: ~60% on API cost alone; we undercut Motion ($228/yr), Inflow (~$200/yr), and coaching ($475–575/mo) with AI inside (price umbrella, competitor-teardowns.md).

## 9) Riskiest assumption + one-month validation

**Riskiest assumption: a forgiving, generated morning brief actually changes the abandonment curve — that users open it often enough, and return after lapses, rather than habituating to the brief itself the way they habituate to alarms.** (Honest corollary flagged by verification.md: severity of these mechanisms is convergent but partly unmeasured; and our anti-habituation claim applies to our own notification too.)

One-month validation, three parallel probes:
1. **Concierge Wizard-of-Oz (weeks 1–4):** recruit 20 ADHD adults (r/ADHD, ADHD Twitter/TikTok). Each morning they receive a human-written (LLM-assisted, human-sent) Anchor brief via SMS/email built from a shared task sheet; evening close likewise. **Pass metrics:** ≥40% still opening the brief at day 21 (vs the 3.3%/30-day category baseline); ≥60% of users who lapse ≥3 days re-engage within 72h of a welcome-back brief; qualitative "I'd pay" from ≥8/20.
2. **Landing-page smoke test (weeks 1–2):** two positionings — "the planner that forgives you" (anchor-first) vs "never lose a project again" (memory-first) — $500 ad spend each; compare email-signup CVR to test whether the forgiveness framing is the stronger acquisition wedge.
3. **Cost telemetry (weeks 2–4):** meter the real token usage of generating the concierge briefs with the production prompt chain to confirm the §8 math before writing app code.

Kill/pivot criterion: if day-21 brief-open is <20% even with human-quality briefs, the anchor thesis is wrong and the correct pivot is toward the project-shelf as primary surface.

## 10) Why we beat the other teams

**vs Execution-first (initiation/"wall of awful" as the wedge).** Three problems. (a) Verification explicitly downgrades their underservedness claim: initiation is "**not fully underserved** — body-doubling category (Focusmate, Flow Club, Dubbii) targets initiation" (verification.md), and Dubbii alone has 500k+ users (raw findings) — they're entering a lane with named incumbents while claiming white space. (b) Their keystone stat is quarantined: the "58% weekly paralysis" figure is ❌ killed, and the 62% survey is 🟡 community-signal only — the evidentiary floor under an initiation-first thesis is the thinnest of the three. (c) Structurally, an execution moment has no *daily surface*: nothing brings the user back tomorrow, so it inherits the 3.3% cliff with no counter-mechanism. Anchor delivers execution support where it actually works — as point-of-performance nudges inside a rhythm the user already receives (Barkley's externalization; collapse of action-reward delay per the [JAD delay-discounting meta-analysis](https://journals.sagepub.com/doi/10.1177/1087054718772138)) — we get their feature set without their retention problem.

**vs Project-memory-first (re-entry as the wedge).** Their wedge is real — verification confirms re-entry as the best-evidenced underserved problem — but verification *also* hands us the argument that beats them: the category's two best-funded memory plays failed exactly this way ("[Napkin shut down](https://napkin.one/); Mem.ai raised $23.5M and is written up as a failure case"), and the binding constraint states the resurfacing product must be "**tied to daily use, not a serendipity lottery**" (verification.md §3). A memory product without a daily container has no moment at which to resurface anything — it *is* the Mem playbook rerun. Moreover, re-entry frequency is admittedly unmeasured (their usage cadence is unknown), while a daily anchor has daily cadence by construction. Anchor ships their best ideas — breadcrumbs, re-entry briefs, the project Shelf — as Module C/E *inside* the surface that gives them a delivery moment. They built the organ; we built the body it needs to live in.

**The judge's summary:** all three teams draw on the same evidence; only the daily anchor converts every strand of it — capture, breakdown, execution, re-entry, shame-safety, anti-habituation, honest billing — into one habit-shaped product with a built-in daily reason to return, a costed path under $5.99/user, and a falsifiable one-month test against the one metric the entire category fails: day 30.
