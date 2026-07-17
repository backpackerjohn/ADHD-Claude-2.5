# Problem Hunt — Merged & Ranked Candidate Problems

**Method.** Eight parallel research agents swept distinct angles (community complaints, app-review complaints, thought capture, task paralysis/projects, time & life admin, competitor landscape, behavioral science, market data) via web-search retrieval (direct fetch blocked by sandbox policy — see `build-log.md` D-002). Raw findings with every quote and URL: `research/raw/*.json`. Adversarial verification verdicts: `research/verification.md`. Ranking criteria: pain intensity × breadth of independent sources × underservedness × suitability for a realistic AI app used in normal daily life (no AI phone calls, no hardware, no exotic systems — Guardrail 1).

---

## The candidate problems, ranked

### P1. The execution gap: task-initiation paralysis ("wall of awful") — severity 5/5
Knowing exactly what needs doing and being physically unable to start. Not procrastination — a neurological freeze wrapped in an emotional wall built from years of failure.
- "ADHD paralysis is sitting on the couch, staring at the dishes, knowing they need to be done, and being physically unable to make yourself get up and do them." — [Inflow](https://www.getinflow.io/post/adhd-couch-lock)
- "Getting Started Blues" survey: mental paralysis reported by **62%** vs 44% procrastination — [ThriveWithADD](https://thrivewithadd.com/paralysis-beats-procrastination-as-problem-for-add-adhd-adults-according-to-getting-started-blues-survey/)
- Covered by [Cleveland Clinic](https://health.clevelandclinic.org/adhd-paralysis), [ADDA](https://add.org/adhd-paralysis/), the ["Wall of Awful"](https://www.adhdessentials.com/essentials/the-wall-of-awful/) framework.
- **Why underserved:** every tool attacks the plan (Goblin Tools "generates a beautiful list of steps and then stops at the plan" — [Thawly](https://thawly.ai/reviews/goblin-tools)) or the scheduled moment (body doubling), not the freeze itself. The emotional layer is only served by $170–225/hr coaches ([Coaching Executive Function](https://www.coachingexecutivefunction.com/post/how-much-does-adhd-coaching-cost)).

### P2. Project re-entry friction + the project graveyard — severity 4/5, **most underserved**
Returning to a paused project after weeks feels like starting from zero; projects stall at 80% and pile up.
- "Projects often die because you lose context—you open the document and can't remember where you were or what you were thinking… the re-entry cost feels enormous." — [Fabric](https://fabric.so/blog/how-to-finish-projects-when-your-brain-keeps-starting-new-ones)
- "Restarting after an interruption may feel like beginning the whole task again. That friction makes restarting harder than starting something new." — [ADHD Philadelphia](https://www.adhdphiladelphia.com/blog/why-adults-with-adhd-lose-momentum-so-easily-after-interruptions)
- "A graveyard of 80%-finished projects because the first 20% offers novelty, the last 20% offers completion's reward, but the middle offers neither." — [Tiimo](https://www.tiimoapp.com/resource-hub/finishing-what-you-start-adhd)
- Real-world cost: spouse describing 2.5 years without a kitchen — [ADHDandMarriage forums](https://www.adhdmarriage.com/content/2-12-years-w-no-kitchen-endless-unfinished-projects-homehelp-me-understand)
- **Why underserved:** paralysis and breakdown have dedicated tools; re-entry has none. Every mitigation (breadcrumb notes, work journals) requires remembering to act at the exact moment focus breaks — the very function that's impaired.

### P3. Thought capture failure + the notes graveyard — severity 5/5
Ideas evaporate before capture; what does get captured scatters into unfindable, never-reviewed graveyards; filing decisions kill capture.
- "When it vanishes, it's like being betrayed by your own mind." — [InFocus First](https://infocusfirst.com/adhd-forgetfulness/)
- "Notion requires an organizational decision at the moment of every save… so the system gets silently abandoned despite an enthusiastic setup. It's a design-fit problem, not a willpower problem." — [MindStash](https://www.mindstash.app/blogs/why-your-adhd-brain-hates-notion-and-what-actually-works-instead)
- "I know I wrote it down somewhere… But is it in my Apple Notes, my Kindle highlights, Scrivener, a random Word document, one of my notebooks…?" — [Passionate Writer Coaching](https://passionatewritercoaching.com/best-free-note-taking-apps/)
- Night dimension: "Up to 70% of people with ADHD report insomnia" — [Eureka Health](https://www.eurekahealth.com/resources/racing-thoughts-at-bedtime-insomnia-adhd-connection-en); "Your brain races because it's afraid of forgetting" — [Built for ADHD](https://www.builtforadhd.com/blog/adhd-brain-dump-method-clear-47-mental-tabs/)
- **Why underserved:** incumbents offload organization onto the user; the startups selling auto-organization (MindStash, Saner.AI) are tiny and don't close the loop to resurfacing at the moment of relevance.

### P4. Time blindness & "waiting mode" — severity 5/5
- One-third of 1,859 surveyed adults said time-management problems contribute the greatest stress to their lives — [ADDitude](https://www.additudemag.com/punctuality-time-blindness-adhd-apps-tips/)
- "Waiting mode": unable to do anything productive in the hours before an appointment — [Healthline](https://www.healthline.com/health/adhd/adhd-waiting-mode)
- **Partially served:** Tiimo/Structured/Time Timer visualize time well. The unserved slices: backwards-planning ("start now to make Y"), waiting-mode-sized task suggestions, felt-time translation.

### P5. Life-admin failure & the ADHD tax — severity 5/5
- 57% miss loan payments; default rates grow exponentially in middle age; **$1.27M lifetime income deficit** — [ADDitude/Reuters](https://www.additudemag.com/adhd-tax-late-fees-fines-shame/), [Science Advances](https://www.science.org/doi/10.1126/sciadv.aba1551)
- 60–90% more likely to miss GP appointments — [University of Bath](https://www.bath.ac.uk/announcements/new-study-reveals-high-rates-of-missed-gp-appointments-among-patients-with-adhd/)
- Involuntary ghosting of texts/emails destroys friendships — [Inflow](https://www.getinflow.io/post/adhd-involuntary-ghosting-texting-friends)
- **Caution:** the full surface (mail, bank accounts, inbox) requires deep integrations — heavy lift for v1, and autopay/budget apps partially serve the bills slice.

### P6. Reminder habituation / notification blindness — severity 4/5 (cross-cutting mechanic)
- Repeated identical tones filtered like background noise within about a week — [Sprout](https://www.sproutapp.tech/blog/adhd-reminder-app), [My Patient Advice](https://mypatientadvice.co.uk/knowledge-base/why-do-adhd-brains-still-ignore-phone-alarms/)
- **Treat as a design requirement for any winning product, not a standalone product.**

### P7. Shame-naive tool design (streaks, overdue walls) — severity 5/5 (cross-cutting)
- Streaks called "the single worst design choice for ADHD users" — [Kabit](https://kabitapp.com/blog/habit-tracker-adhd); Todoist's "task graveyard" shame loop — [Thawly](https://thawly.ai/reviews/todoist-for-adhd)
- Median 30-day retention for mental-health apps ≈ **3.3%** — [JMIR](https://mhealth.jmir.org/2020/11/e16309/)
- **Treat as a design requirement:** the product must assume absence and design for guilt-free return.

### P8. Partner mental load — severity 5/5, adjacent
- 96% of ~700 surveyed spouses say symptoms make household management harder — [WebMD](https://www.webmd.com/add-adhd/adult-adhd-marriage)
- **Deferred:** two-sided product; better as a later expansion than a v1 target.

---

## Cross-cutting truths every thesis must respect

1. **The list was never the problem.** Capture/planning tools abound; execution, re-entry, and resurfacing are the unserved verbs. ([Thawly](https://thawly.ai/reviews/goblin-tools), [Lifestack](https://lifestack.ai/blog/best-ai-assistants-for-adhd))
2. **Any demanded consistency will not be supplied.** Design for inconsistent engagement: zero-setup day one, graceful degradation, shame-free re-entry. ([Tiimo resource hub](https://www.tiimoapp.com/resource-hub/why-productivity-systems-fail-adhd))
3. **Externalize at the point of performance** (Barkley): cues, time, and motivation delivered at the moment of action, not in a list reviewed elsewhere. ([Barkley factsheet](https://www.russellbarkley.org/factsheets/ADHD_EF_and_SR.pdf))
4. **Collapse delay between action and reward** (delay-discounting meta-analysis — [JAD](https://journals.sagepub.com/doi/10.1177/1087054718772138)).
5. **Fight habituation with novelty** (novelty-processing research — [Brain](https://academic.oup.com/brain/article/141/5/1545/4934119)).
6. **Honest pricing is a trust wedge.** The community loudly resents predatory subscriptions and cancellation dark patterns. ([Future ADHD](https://futureadhd.com/articles/adhd-product-scammer-callout/), [Choosing Therapy on Inflow](https://www.choosingtherapy.com/inflow-adhd-app-review/))
