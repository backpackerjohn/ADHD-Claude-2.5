# Thesis A — Execution-First

## 1) Product name + one-line pitch

**Ignition** — the ADHD app that gets you *into motion* in the next five minutes: it meets you at the moment of freeze, hands you a two-minute first move, sits with you while you do it, and pays you dopamine on the spot.

One-line for the store page: *"Not another list. A starter motor."*

---

## 2) Target user

**"Frozen Fran," 32, diagnosed at 28.** Knowledge worker or grad student, one of the ~15.5M US adults (6.0%) with a current ADHD diagnosis, roughly half diagnosed in adulthood ([CDC MMWR 73:890-895](https://www.cdc.gov/mmwr/volumes/73/wr/mm7340a1.htm)). She owns a phone full of "digital ghosts" — planners abandoned by day three ([Productive with Chris](https://productivewithchris.com/guides/best-planning-apps-adhd-2025/)). She does not lack a plan; she has three plans in three apps. Her actual failure point is 2:47pm on a Tuesday: staring at the task, "sitting on the couch, staring at the dishes, knowing they need to be done, and being physically unable to make yourself get up and do them" ([Inflow](https://www.getinflow.io/post/adhd-couch-lock)). She has looked at coaching and body doubling: coaching runs $170–225/session and is rarely insured ([Coaching Executive Function](https://www.coachingexecutivefunction.com/post/how-much-does-adhd-coaching-cost), [NeuroLaunch](https://neurolaunch.com/is-adhd-coaching-covered-by-insurance/)); Focusmate requires scheduling a camera-on session with a stranger — itself an initiation barrier ([Medical News Today roundup](https://www.medicalnewstoday.com/articles/body-doubling-adhd)). She will disappear from any app for two weeks at a time; the winning product must assume that and welcome her back.

[INFERENCE] Persona details (age, job) are a composite; the traits above each trace to the cited evidence.

---

## 3) Winning problem + evidence

**The execution gap: task-initiation paralysis, ranked severity 5/5 and #1 in our problem hunt.** Knowing exactly what needs doing and being neurologically unable to start.

- "ADHD paralysis is sitting on the couch, staring at the dishes, knowing they need to be done, and being physically unable to make yourself get up and do them." — [Inflow](https://www.getinflow.io/post/adhd-couch-lock)
- "The Wall of Awful is the emotional barrier that grows out of repeated failure... because those with ADHD fail more often than others, usually in the same or similar ways, their walls tend to be larger." — [Hacking Your ADHD (Brendan Mahan)](https://www.hackingyouradhd.com/podcast/the-wall-of-awful-with-brendan-mahan)
- Clinically recognized across [Cleveland Clinic](https://health.clevelandclinic.org/adhd-paralysis), [ADDA](https://add.org/adhd-paralysis/), and coaching frameworks. Community signal (self-run coach survey, methodology unpublished — cite as signal only per verification.md): mental paralysis reported by 62% vs 44% procrastination ([ThriveWithADD](https://thrivewithadd.com/paralysis-beats-procrastination-as-problem-for-add-adhd-adults-according-to-getting-started-blues-survey/)).

**Why the gap is open despite a crowded market.** Every incumbent stops one step short of motion:

- Breakdown tools stop at the list: Magic ToDo "generates a beautiful list of steps and then that's it — it hands you the list and walks away. For many ADHD users, the list was never the problem. It's a planning tool, not an execution tool." ([Thawly](https://thawly.ai/reviews/goblin-tools)) And "each session starts fresh" — no memory, no personalization ([FocusHack](https://www.focushack.io/reviews/goblin-tools-adhd-review/)).
- Planners stop at display: "if your bottleneck is execution rather than scheduling, you will need something else alongside them" ([Lifestack](https://lifestack.ai/blog/best-ai-assistants-for-adhd)). (Directionally-true caveat per verification.md: this narrative circulates heavily on competitor blogs — the wedge is contested, which is why speed and depth matter.)
- Body doubling attacks the scheduled moment, not the unscheduled freeze, and carries its own initiation barrier ([Medical News Today](https://www.medicalnewstoday.com/articles/body-doubling-adhd)). Verification.md is explicit that Focusmate/Flow Club/Dubbii make initiation "not fully underserved" — our answer is that they serve *pre-booked* initiation; the freeze is unscheduled by nature.
- The emotional layer is served only by $170–225/hr coaches ([Coaching Executive Function](https://www.coachingexecutivefunction.com/post/how-much-does-adhd-coaching-cost)).

**Supporting science (the mechanics we build on):**
- Externalize cues, time, and motivation *at the point of performance*, not in a list reviewed elsewhere ([Barkley factsheet](https://www.russellbarkley.org/factsheets/ADHD_EF_and_SR.pdf)).
- Collapse the delay between action and reward — delay-discounting meta-analysis ([JAD](https://journals.sagepub.com/doi/10.1177/1087054718772138)).
- Implementation intentions ("when X, I will Y") show d = .65 on goal attainment across 94 tests ([Gollwitzer & Sheeran 2006](https://www.socmot.uni-konstanz.de/publications/implementation-intentions-and-goal-achievement-meta-analysis-effects-and-processes); general population, not ADHD-specific).
- Fight reminder habituation with novelty ([Brain](https://academic.oup.com/brain/article/141/5/1545/4934119); [Sprout](https://www.sproutapp.tech/blog/adhd-reminder-app)).
- Design for absence: median 30-day retention for mental-health apps is ~3.3% ([Baumel et al. 2019, JMIR e14567](https://www.jmir.org/2019/9/e14567/)); streak mechanics backfire for ADHD users ([Klarity](https://www.helloklarity.com/post/breaking-the-chain-why-streak-features-fail-adhd-users-and-how-to-design-better-alternatives/), [medRxiv preprint](https://www.medrxiv.org/content/10.1101/2024.12.26.24319676.full.pdf)).

---

## 4) Core loop (the daily habit)

The loop is engineered so that the *app visit itself* is the low-friction act, and everything after it is downhill. Target: freeze → first physical action in under 90 seconds.

1. **Open to one question, not a list.** Ignition never opens on a backlog. It opens on a single card: "What's in front of you right now?" with three answers: *[a thing I'm avoiding]*, *[pick up a project]* (pulls the top re-entry card), or *[just capture something]*. No overdue counts anywhere. (Anti-graveyard: [Thawly on Todoist](https://thawly.ai/reviews/todoist-for-adhd).)
2. **Shrink to the first physical move.** Whatever she picks, the AI returns exactly ONE two-minute, physically concrete first action ("open the tax folder and count the documents — that's it"), with a "smaller / different / why is this hard?" control. The "why is this hard?" branch runs a 3-exchange emotional-state-first script (Wall-of-Awful informed: name the feeling → shrink the stakes → pick the tiny move) — the layer currently priced at $170–225/hr.
3. **Start a Momentum Session.** One tap starts a 10-minute session: warm on-screen companion presence, ambient check-ins, task steps queued one at a time. No video, no calls, no strangers.
4. **Instant micro-reward on every micro-step.** Completion fires immediate, variable, novelty-rotating rewards (visuals, sounds, progress physics) the moment a step is checked — collapsing the action→reward delay ([JAD meta-analysis](https://journals.sagepub.com/doi/10.1177/1087054718772138)).
5. **Auto-breadcrumb on exit.** When the session ends — or she just vanishes mid-task — Ignition automatically snapshots state (step reached, what she typed, what's next) with zero user effort. This is what makes tomorrow's step 1 possible, because manual breadcrumbing "depends on the very executive function that's impaired" ([r/ADHD_Programmers via search summary](https://www.reddit.com/r/ADHD_Programmers/); [ADHD Philadelphia](https://www.adhdphiladelphia.com/blog/why-adults-with-adhd-lose-momentum-so-easily-after-interruptions)).
6. **Tomorrow (or in three weeks): a welcome, not a wall.** Next open shows "Welcome back. Last time you got through step 3 of Taxes. The next move is a 2-minute one." Absence of any length lands on amnesty, never debt. Cumulative "engine hours" replace streaks ([Klarity's recovery-mechanics recommendation](https://www.helloklarity.com/post/breaking-the-chain-why-streak-features-fail-adhd-users-and-how-to-design-better-alternatives/)).

Habit anchor: one optional daily "ignition window" notification at a user-chosen trigger moment, with rotating modality/copy to fight notification blindness ([Sprout](https://www.sproutapp.tech/blog/adhd-reminder-app)). One per day, never a nag stack.

---

## 5) Core modules

**M1 — The Unfreezer (freeze-moment intervention).** The beating heart. Reachable from a lock-screen/home-screen widget in one tap: paste or say the thing you're avoiding, get back one two-minute physical first action, resized on demand (Goblin-style spiciness, which proved demand for AI breakdown — [teardown](https://thawly.ai/reviews/goblin-tools)) — but unlike Goblin, it knows your history, your project context, and what worked to unstick you last time, and it doesn't hand you the list and walk away: its output is a *launch*, not a plan. Includes the "why is this hard?" emotional-first branch. This is Barkley's point-of-performance externalization made literal.

**M2 — Momentum Sessions (synthetic body double + micro-reward engine).** Timed focus sessions with companionship affordances and none of body doubling's barriers: an ambient presence (animated companion + occasional short, warm, context-aware text check-ins: "still on the emails? want a smaller bite?"), variable-interval so it doesn't habituate. No video calls, no AI phone calls, no strangers, no scheduling. Every completed micro-step pays out instantly from a rotating reward pool; session end produces a "what you did" receipt (evidence against the shame narrative). The middle-60% problem — "the first 20% offers novelty, the last 20% offers completion's reward, but the middle offers neither" ([Tiimo resource hub](https://www.tiimoapp.com/resource-hub/finishing-what-you-start-adhd)) — is exactly what a synthetic reward schedule fixes.

**M3 — Sparks (thought capture that serves execution).** *[Founder mandate 1]* One universal capture box — text or voice, zero filing decisions at save time, because "Notion requires an organizational decision at the moment of every save… so the system gets silently abandoned" ([MindStash](https://www.mindstash.app/blogs/why-your-adhd-brain-hates-notion-and-what-actually-works-instead)). AI auto-tags and auto-attaches each spark to the relevant project or a triaged holding area. Crucially, capture is execution-facing: a spark that looks like a task is offered back as a candidate first move; a 2am racing thought (insomnia hits 66.8% of ADHD adults vs 28.8% controls — [Brevik et al.](https://pubmed.ncbi.nlm.nih.gov/28547881/)) gets a "captured — it will be there tomorrow, you can let it go" night mode. Sparks decay gracefully into a searchable archive instead of a guilt pile.

**M4 — The Project Shelf + Re-entry Cards.** *[Founder mandates 2 & 3]* Big goals ("do my taxes," "job hunt," "finish the kitchen") get an AI-laddered breakdown workable across a year: milestone → this-month chunk → this-week bite → today's two-minute first move; the ladder re-plans when reality changes, since one-shot breakdowns "don't know your deadlines, documents, or energy" ([raw findings on Goblin's gap](https://thawly.ai/reviews/goblin-tools)). The Shelf is a visual, browsable, searchable home for every project and thought-cluster — view it, pick anything up, find anything. Picking up a dormant project deals a **re-entry card** built from auto-breadcrumbs: "here's where you were, here's what you were thinking, here's the tiny next step" — the exact artifact the research says nobody generates ([Fabric](https://fabric.so/blog/how-to-finish-projects-when-your-brain-keeps-starting-new-ones), [ADHD Philadelphia](https://www.adhdphiladelphia.com/blog/why-adults-with-adhd-lose-momentum-so-easily-after-interruptions)). Projects can also be *consciously shrunk or honorably killed* ("finish, shrink, or kill") so the graveyard never forms ([raw findings gap](https://www.tiimoapp.com/resource-hub/finishing-what-you-start-adhd)).

**M5 — Shame-Safe Fabric + Honest Billing (system-wide, evidence-mandated).** No streaks, no overdue counts, no red states; auto-amnesty converts stale items to "resting" silently; bounce-back metrics celebrate returns. Billing: one-tap in-app cancellation, pause-instead-of-cancel, trial-expiry warnings, and an automatic "you haven't used Ignition in 30 days — want to pause payments?" email — inverting the category's dark patterns ([Choosing Therapy on Inflow](https://www.choosingtherapy.com/inflow-adhd-app-review/), [Trustpilot on Tiimo](https://www.trustpilot.com/review/tiimo.dk), [ADHD-Friendly on the subscription ADHD tax](https://www.adhdfriendly.com/when-adhd-meets-paid-subscriptions-the-struggle-to-keep-up/)). This is a trust wedge no incumbent ships.

---

## 6) Why AI is necessary — and what deliberately is NOT AI

**Necessary because the core interactions are irreducibly contextual and linguistic:**
- Turning "I need to deal with my taxes and I feel sick about it" into one concrete, correctly-sized physical action requires understanding the task, the user's history, and their emotional state — a lookup table can't do it, and Goblin proved the generic version isn't enough ("doesn't know your deadlines, documents, or energy").
- The emotional-first unstick script is a conversation. The only non-AI supply of this is $170–225/hr humans.
- Auto-filing sparks without user decisions, and compressing session activity into a warm re-entry briefing, are summarization/classification tasks — LLM home turf.
- Year-long ladder re-planning when life changes is contextual re-decomposition.

**Deliberately NOT AI (reliability, cost, and trust):**
- Timers, session mechanics, reward animations and payout schedules — deterministic (rewards must fire in <300ms; an API round-trip would reintroduce the delay we exist to collapse).
- Notification scheduling and modality rotation — rule-based.
- Breadcrumb *capture* — automatic event logging, no model call; only the *briefing* rendered from it uses AI.
- Progress visuals, search, the Shelf, all navigation — conventional software.
- Nothing agentic touches the user's calendar, email, or money in v1. No auto-scheduling: Motion shows that maximal AI scheduling produces oppressive plans ([Saner.AI Motion roundup](https://www.saner.ai/blogs/motion-reviews)).

---

## 7) Differentiation

| Player | What they own | Why Ignition wins the execution moment |
|---|---|---|
| **Tiimo** ($12/mo, App of the Year) | Today's visual schedule | Tiimo "shows a *plan*, not a *path into motion*" — no project memory, no thought capture, no freeze-moment support ([teardown](https://daringfireball.net/2025/12/2025_app_store_award_winners)). Its cracks (failing reminders — [JustUseApp](https://justuseapp.com/en/app/1480220328/tiimo-visual-daily-planner/reviews); "almost impossible" cancellation — [Trustpilot](https://www.trustpilot.com/review/tiimo.dk)) are our M5 wedge. We complement schedules; we don't fight for the calendar. |
| **Goblin Tools** (free/$3.49) | One-shot AI breakdown | "Stops at the plan… walks away" ([Thawly](https://thawly.ai/reviews/goblin-tools)); "each session starts fresh" ([FocusHack](https://www.focushack.io/reviews/goblin-tools-adhd-review/)). Ignition = Goblin's beloved moment + memory + companionship + reward + re-entry. Goblin validated demand and taught us the price ceiling for a memoryless tool. |
| **Motion** ($19/mo) | AI auto-scheduling | "Optimizes for fitting everything in" → oppressive schedules; heavy onboarding ([Saner.AI](https://www.saner.ai/blogs/motion-reviews), [Morgen on pricing](https://www.morgen.so/blog-posts/motion-pricing)). We automate *initiation*, not calendars — the opposite bet, fit to the evidence that demanded consistency will not be supplied. |
| **Saner.AI** ($8–16/mo, ~5 people) | "Jarvis for ADHD" notes-first assistant | Notes-first, embryonic (~$120K pre-seed — [OpenTools](https://opentools.ai/tools/sanerai)). Its center of gravity is knowledge; ours is motion. The AI-native wave is "single-slice… mostly unproven" ([teardowns](https://www.saner.ai/blogs/motion-reviews)); we bundle the lifecycle capture → breakdown → execution → re-entry, which "is claimed by no one." |

**Why the Mem/Napkin failures don't apply.** Mem ($23.5M, written up as a failure case) and Napkin (desktop app shut down — [napkin.one](https://napkin.one/)) bet on *passive resurfacing of notes* — a serendipity lottery with no daily job. Verification.md's steelman is our spec: Ignition resurfaces **commitments and projects with a concrete next action**, inside a daily-use execution habit (the Unfreezer/Momentum loop is why you open the app; re-entry cards ride on that visit). Notes here are fuel for action, never the product.

---

## 8) Cost & pricing sketch

**Price: $8.99/mo or $69/yr** (annual ≈ $5.75/mo) — under Tiimo's $12, far under Inflow's ~$22.49/mo self-serve and Motion's $228/yr, inside the open $6.99–9.99 umbrella the teardowns identify. Free tier: Unfreezer 3×/day + capture, forever (Goblin taught the top-of-funnel play).

**Model tiers.** Cheap fast model (Haiku-class, ~$1/M input, $5/M output) for all routine calls; mid-tier model (Sonnet-class, ~$3/M in, $15/M out) only for the two jobs where synthesis quality is the product. No frontier-class calls in the loop.

**High-usage user (aggressive: uses Ignition hard every day, 30 days/mo):**

| Call | Tier | Volume | Tokens (in/out per call) | Monthly tokens | Cost |
|---|---|---|---|---|---|
| Unfreezer breakdowns + resizes | cheap | 8/day | 900 / 350 | 216K / 84K | $0.64 |
| Emotional unstick scripts | cheap | 1/day (3 exchanges) | 3,600 / 900 | 108K / 27K | $0.24 |
| Momentum-session check-ins | cheap | 2 sess/day × 5 msgs | 800 / 80 each | 240K / 24K | $0.36 |
| Spark auto-filing/triage | cheap | 12/day | 400 / 80 | 144K / 29K | $0.29 |
| Re-entry briefing / daily welcome | mid | 1/day | 3,500 / 500 | 105K / 15K | $0.54 |
| Project ladder (re)planning | mid | 2/week | 5,000 / 1,200 | 40K / 10K | $0.27 |
| Weekly Shelf digest | mid | 1/week | 6,000 / 800 | 24K / 3K | $0.12 |
| **Total** | | | | **~0.88M in / 0.19M out** | **≈ $2.46/mo** |

Prompt caching on the static system/persona prompts (the bulk of input tokens) realistically cuts this toward **~$1.60–2.00**; even at 2× the modeled usage we sit ≈ $4.90 — under the $5.99 ceiling with headroom. Median users (remember: category median 30-day retention is 3.3% — [Baumel 2019](https://www.jmir.org/2019/9/e14567/)) will cost far less, so blended margin is comfortable at $8.99. [INFERENCE: token sizes are engineering estimates; per-MTok rates are current published cheap/mid-tier pricing bands.]

Honest-billing costs (pause prompts, 30-day-unused emails) sacrifice some short-term revenue deliberately; the category's Trustpilot record says trust compounds into retention and word-of-mouth in this community ([Future ADHD callout](https://futureadhd.com/articles/adhd-product-scammer-callout/)).

---

## 9) Riskiest assumption + one-month validation

**Assumption: users will actually reach for the app at the moment of freeze.** Everything downstream (breakdown quality, rewards, re-entry) is moot if the frozen user doesn't open Ignition — freeze might suppress *app-opening* exactly as it suppresses dish-washing. (Honesty per verification.md: paralysis frequency stats are community-signal grade, and the 3.3% retention base rate is brutal.)

**Four-week validation plan:**
- **Week 1:** Landing-page smoke test, two framings ("get unstuck in 2 minutes" vs. control "AI ADHD planner"); success = execution framing beats control on waitlist conversion by ≥50%. Plus 15 interviews probing: "the last time you were frozen, what did you physically do with your hands/phone?"
- **Weeks 2–4:** Concierge Wizard-of-Oz with 25–30 recruits from ADHD communities: a bare-bones web capture box + one-tap "I'm stuck" widget; a human operator (scripted, later swapped for the LLM prompt verbatim) returns the two-minute first move and text check-ins. **Kill/scale metrics:** (a) ≥40% of participants self-initiate an "I'm stuck" request in ≥3 distinct weeks; (b) ≥50% of unstick interventions lead to self-reported task start within 10 minutes; (c) week-4 return rate ≥25%. Miss (a) → pivot the entry point from pull to a scheduled "ignition window" push and retest before building.

This doubles as prompt R&D: every operator transcript becomes eval data for the real system.

---

## 10) Why we beat the other teams

**vs. project-memory-first.** Re-entry is real and underserved — we agree, and we ship it (M4 is a first-class module). But memory-as-the-product has two structural flaws the verification file itself flags: (1) severity is "asserted anecdotally, never measured," and (2) the two best-funded resurfacing bets, Mem and Napkin, already failed because resurfacing without a daily job has no habit to live in. Re-entry is an *every-few-weeks* event; you cannot build a daily-retention product on it. Ignition gets the ordering right: the daily execution loop earns the visits, breadcrumbs accrue automatically as its exhaust, and re-entry cards deliver the memory payoff *inside* an existing habit. A memory-first rival must convince a judge that users will return to a memory app they haven't needed for three weeks — against a 3.3% category retention base rate. We don't have to.

**vs. daily-anchor-first.** The daily-planner lane is the most saturated in the category, with a breakout incumbent (Tiimo, Apple's 2025 iPhone App of the Year) and a defining failure mode on record: Sunsama's ritual "requires its own executive function to initiate… the ritual can become 'another thing to fail at'" ([rivva](https://blog.rivva.app/p/best-sunsama-alternatives-for-adhders), [Business Dive](https://thebusinessdive.com/sunsama-review)); "mainstream productivity systems assume consistent energy… assumptions that almost never hold for ADHD" ([Tiimo's own resource hub](https://www.tiimoapp.com/resource-hub/why-productivity-systems-fail-adhd)). An anchor ritual is a consistency demand, and the research's second commandment is that demanded consistency will not be supplied. Ignition requires zero daily ritual: it works the first time you're stuck, works after three weeks away, and coexists with whatever planner the user already loves rather than fighting Tiimo on its home field.

**The positive case.** Ignition is built exactly where the evidence converges: the #1-ranked, severity-5 problem (initiation paralysis); the unclaimed verbs ("execution and re-entry are unclaimed" — competitor teardowns); the three strongest confirmed mechanisms (point-of-performance externalization, delay-collapse rewards, implementation intentions d=.65); and the two confirmed trust wedges (shame-safety, honest billing) shipped as core modules rather than marketing. It honors every founder mandate — thought organization (M3), year-scale breakdown (M4), and browse/pick-up/find (Shelf + re-entry cards) — while keeping them in service of the one metric that defines the category's failure and our success: **did the user start moving today?** The list was never the problem. Motion is. We sell motion.
