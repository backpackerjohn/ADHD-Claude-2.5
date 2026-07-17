# Ember — Brand Guidelines

> **Definition of done for this file:** a stranger with no other context can produce a new on-brand asset (screen, notification, email, App Store screenshot, social card) from this document alone. Terminology follows `product/product-brief.md`; tone constraints follow `contest/winner.md`.
>
> **Version 1.0 · 2026-07-17 · Owner: design lead**

---

## 1. Brand story & personality

### 1.1 The story in one breath

Ember is the app that keeps your projects warm. An ember is what a fire becomes when nobody is tending it — smaller, quieter, **still alive**. Come back after a day or after two months, cup your hands around it, and it re-kindles. That is the entire brand: your thoughts and projects don't die when you look away. Nothing is lost.

### 1.2 Who we are talking to

Maya, 34, diagnosed with ADHD at 31. She owns a project graveyard — novel at 60%, half-built Etsy shop, a two-color room. Every productivity app she has tried eventually became a mirror of her failures: red badges, overdue counts, dead streaks. Ember's job is to be the first tool in her life that gets *better* when she disappears, and greets her return with a briefing instead of a bill.

### 1.3 Personality: the warm hearth

Ember speaks like a trusted friend who kept notes for you while you were away — and is genuinely glad you're back.

| We are | We are never |
|---|---|
| **Warm** — a hearth, a kitchen at night, a hand on the shoulder | **Clinical** — no lab whites, no medical blues, no "symptom", "deficit", "disorder", "executive function" in UI copy |
| **Calm** — small surfaces, one action, generous quiet space | **Productivity-bro** — no "crush it", "grind", "10x", "optimize your life", no dashboards bristling with graphs |
| **Steady** — memory that survives absence; the copy never flinches when the user lapses | **Infantilizing** — no baby-talk, no cutesy mascots cheering, no gold-star kindergarten rewards, no "you did a thing!!" |
| **Honest** — plain words about price, pauses, and what the AI did | **Manipulative** — no guilt, no FOMO, no streak-loss threats, no retention mazes |
| **Quietly witty** — a small smile, never a punchline at the user's expense | **Sarcastic or edgy** — the user's shame is off-limits as material |

**Litmus test for any asset:** would this feel right glowing softly on a phone at 1 a.m. next to someone who hasn't opened the app in six weeks? If it would make that person wince, it's off-brand.

### 1.4 Brand metaphor rules

- Warmth = recency. Warm things are recent and alive; cool things are resting, not dead.
- Fire vocabulary is allowed at *ember* scale: glow, warm, spark, kindle, rekindle, resting, banked. **Never** blaze, burn, fire-under-you, burnout jokes, or explosion imagery.
- Absence is neutral-to-tender, never a debt. "Resting" is a shelf, not a penalty box.

---

## 2. Name & wordmark

### 2.1 The name

**ember** — always lowercase in the wordmark and in UI self-reference ("ember filed this"). In prose, sentence rules apply ("Ember keeps your projects warm."). Never EMBER, never camel-case, never "the Ember app" in-product.

### 2.2 Wordmark construction (text-based; no drawn logo required)

The wordmark is typeset, so any designer or developer can rebuild it exactly:

1. **Type** the word `ember` in **Fraunces SemiBold (600)**, optical size 72+ if using the variable font, lowercase, letter-spacing **−1%** (−0.01em). Fallback if Fraunces is unavailable: Georgia Bold — but ship Fraunces for brand assets.
2. **Color:** Ink `#231A12` on light surfaces; Cream `#F4EBDD` on dark surfaces. The wordmark letters are never orange.
3. **The spark accent:** a single soft dot floating above the ascender of the **b** — the only ascender in the word, so the spark reads as a fleck rising from the fire.
   - Shape: a circle with a radial gradient — center Amber `#F5B942`, mid Ember `#E85D0F` at 60%, fading to transparent at the edge. (Solid `#E85D0F` circle is the acceptable flat/1-color fallback.)
   - Size: diameter = **0.28 × x-height** of the wordmark.
   - Position: horizontally centered on the b's ascender stem; vertically, its center sits **0.35 × x-height above** the ascender's top. It floats — it must never touch the letter.
4. **Lockup with tagline** (optional, marketing only): tagline "keeps your projects warm" in the UI sans stack, Regular, 0.28 × wordmark cap height, letter-spacing +2%, color Secondary text, set one x-height below the baseline, left-aligned to the e.

**Spark-only mark** (app icon, favicon, avatars): the radial spark centered on an Ink `#231A12` rounded square (radius 22% of side), spark diameter 45% of side. This is the only context where the spark appears without the wordmark.

### 2.3 Clearspace & minimum sizes

- **Clearspace:** on all four sides, keep a margin equal to the height of the lowercase **e** (the x-height). Nothing — text, edges, other logos — enters this zone. The spark is part of the wordmark; measure clearspace from the spark's top.
- **Minimum sizes:** wordmark ≥ **72 px wide** (digital) / **20 mm** (print). Below that, use the spark-only mark, minimum **16 px**.

### 2.4 Wordmark don'ts

- Don't set the letters in orange, gradients, or outlines.
- Don't move the spark to another letter, add more sparks, or animate it in exported logos (in-app, a ≤300 ms glow-in on launch is permitted).
- Don't capitalize, stretch, condense, rotate, add drop shadows, or place on busy photography without an Ink or Cream scrim.
- Don't pair with flame clip-art, rockets, checkmarks, or brains.
- Don't recreate in another typeface for "consistency" with a partner's system — ship the files.

---

## 3. Color system

Warm-black hearth darkness, cream paper, and one ember. **Orange is THE accent** — if everything glows, nothing is warm.

### 3.1 Core palette

| Name | Hex | Role |
|---|---|---|
| **Ink** (warm black) | `#231A12` | Text on light; dark-theme is built from its family |
| **Cream** | `#FAF4E8` | Light app background |
| **Paper** | `#FFFDF8` | Light cards/sheets (elevated) |
| **Parchment** | `#F1E7D6` | Light sunken/resting surfaces |
| **Ember** (core accent) | `#E85D0F` | THE accent: primary actions, the spark, warmth cues |
| **Ember Deep** | `#B0400A` | Text-safe ember on light; pressed states |
| **Amber** | `#F5B942` | Glow gradients, warm chips, "warm" indicators |
| **Terracotta** | `#C06A4D` | Muted secondary warmth (graphic) · text-safe deep: `#9A4B33` |
| **Sage** | `#7E9670` | Success/done/growth (graphic) · text-safe deep: `#4A6141` · mist: `#DCE5D4` |
| **Dusty Blue** | `#7189B0` | Rest, snooze, night, resting shelf (graphic) · text-safe deep: `#44577C` · mist: `#DDE4EF` |

**Neutrals (light):** text secondary `#5C4F42` · text tertiary `#7A6A58` · border `#E5D9C6` · hairline `#EFE6D6`.

### 3.2 Dark theme mapping

Dark mode is a **banked hearth**, not inverted gray. Every neutral keeps the warm cast.

| Role | Light | Dark |
|---|---|---|
| App background | Cream `#FAF4E8` | Hearth `#181210` |
| Card / sheet | Paper `#FFFDF8` | `#221B16` |
| Sunken / resting | Parchment `#F1E7D6` | `#2B231C` |
| Text primary | Ink `#231A12` | `#F4EBDD` |
| Text secondary | `#5C4F42` | `#C2B4A3` |
| Text tertiary | `#7A6A58` | `#94867A` |
| Accent (interactive) | Ember `#E85D0F` (fills) / Ember Deep `#B0400A` (text) | Ember Bright `#FF8E3D` |
| Text on accent fill | Ink `#231A12` | Hearth `#181210` |
| Success | Sage Deep `#4A6141` | Sage Bright `#A5BC97` |
| Rest / snooze | Dusty Deep `#44577C` | Dusty Bright `#9FB4D8` |
| Muted warmth | Terracotta Deep `#9A4B33` | Terracotta Bright `#E08A6D` |
| Border | `#E5D9C6` | `#3A3028` |

**Night capture surface** (22:00–06:00 Catch): pure black `#000000` background, text `#B8A28A`, dim text `#8A7660`, no accent fills — a single hairline `#2A2118`. Silent, OLED-true, designed to not wake a partner.

### 3.3 Verified WCAG contrast pairs (WCAG 2.1 relative-luminance math)

| Pair | Ratio | Passes |
|---|---|---|
| Ink `#231A12` on Cream `#FAF4E8` | **15.61 : 1** | AAA |
| Secondary `#5C4F42` on Cream | **7.23 : 1** | AAA |
| Tertiary `#7A6A58` on Cream | **4.76 : 1** | AA |
| Ember Deep `#B0400A` on Cream | **5.35 : 1** | AA |
| Ink on Ember `#E85D0F` (primary button) | **4.89 : 1** | AA |
| Cream `#FAF4E8` on Ember Deep `#B0400A` | **5.35 : 1** | AA |
| Sage Deep `#4A6141` on Cream | **6.23 : 1** | AA (AAA large) |
| Dusty Deep `#44577C` on Cream | **6.61 : 1** | AA (AAA large) |
| Terracotta Deep `#9A4B33` on Cream | **5.59 : 1** | AA |
| Ink on Amber `#F5B942` | **9.69 : 1** | AAA |
| Cream text `#F4EBDD` on Hearth `#181210` | **15.69 : 1** | AAA |
| Dark secondary `#C2B4A3` on Hearth | **9.14 : 1** | AAA |
| Dark tertiary `#94867A` on Hearth | **5.25 : 1** | AA |
| Ember Bright `#FF8E3D` on Hearth | **8.11 : 1** | AAA |
| Hearth text on Ember Bright (dark primary button) | **8.11 : 1** | AAA |
| Cream text on dark card `#221B16` | **14.38 : 1** | AAA |
| Sage Bright `#A5BC97` on Hearth | **9.04 : 1** | AAA |
| Dusty Bright `#9FB4D8` on Hearth | **8.82 : 1** | AAA |
| Night text `#B8A28A` on Black | **8.57 : 1** | AAA |
| Night dim `#8A7660` on Black | **4.84 : 1** | AA |

**Known limits (rules, not exceptions):**
- Ember `#E85D0F` on Cream is **3.19 : 1** — legal only for large text (≥24 px / 19 px bold), icons, and graphics (AA large/graphical = 3:1). Body-size ember text on light must use Ember Deep `#B0400A`.
- White on Ember is **3.50 : 1** — never use white text on ember fills; primary buttons use **Ink on Ember** (4.89:1).

### 3.4 Color usage laws

1. **One ember per screen.** The accent marks the single primary action or the warmth indicator — never both competing, never scattered.
2. **No red anywhere.** Errors use Terracotta Deep `#9A4B33` with plain-language copy. Destructive confirms use Ink-weight typography, not alarm color.
3. Warmth gradient encodes recency: warm Threads carry an amber→ember glow; resting Threads sit on Parchment/Dusty; retired Threads are neutral with a sage "finished" note. Never encode by color alone — always pair with a text label ("warm", "resting").
4. Sage is for *completion and growth*, dusty blue for *rest and night*. Don't swap them; the emotional mapping is load-bearing (rest must not look like failure or like success — it looks like evening).

---

## 4. Typography

### 4.1 Families

| Role | Stack | Why |
|---|---|---|
| **UI / body** (system-first, zero download) | `-apple-system, BlinkMacSystemFont, "Segoe UI", Roboto, "Helvetica Neue", Arial, "Noto Sans", sans-serif` | Instant render — capture must feel native-fast; sub-300ms shells can't wait on webfonts |
| **Display serif** (warmth) | `"Fraunces", "Iowan Old Style", "Palatino Linotype", Georgia, "Times New Roman", serif` | Fraunces (open-source, variable, SOFT axis) is the hearth voice: wordmark, greetings, briefing headlines, Closing Notes, spark quotes |
| **Numeric** | UI stack with `font-variant-numeric: tabular-nums` for timers/ember-hours | No layout jitter |

Fraunces loads `swap` and only on surfaces that read (Doorway, Briefings, Shelf) — never on the Catch overlay, which is 100% system font. Serif = the app talking warmly or quoting *you*; sans = the app doing work. Never set long body paragraphs in the serif.

### 4.2 Scale (1rem = 16px), weights, line heights

| Token | Size | Weight | Line height | Use |
|---|---|---|---|---|
| `display` | 40px / 2.5rem | Serif 600 | 1.15 | Marketing, onboarding hero |
| `title-1` | 32px / 2rem | Serif 600 | 1.2 | Briefing headline, "Welcome back." |
| `title-2` | 24px / 1.5rem | Serif 600 | 1.25 | Card titles, Thread names |
| `title-3` | 20px / 1.25rem | Sans 600 | 1.3 | Section heads, modal titles |
| `body-lg` | 18px / 1.125rem | Sans 400 | 1.55 | Briefing body, Doorway copy |
| `body` | 16px / 1rem | Sans 400 | 1.55 | Default. **Never smaller for reading copy** |
| `body-sm` | 14px / 0.875rem | Sans 400 | 1.45 | Metadata, timestamps, helper text |
| `caption` | 12px / 0.75rem | Sans 500 | 1.35 | Chips, nav labels only — never sentences |
| `quote` | 17px / 1.0625rem | Serif 400 italic | 1.55 | Spark quotes ("your own words") |

Weights available: 400, 500 (medium emphasis), 600 (headings/buttons). Never 300 (too faint for AA at small sizes) and never 700+ (shouty). Buttons: sans 600, 16px. Letter-spacing: 0 for body; +2% for the rare all-caps caption (avoid all-caps generally).

### 4.3 Typographic rules

- Measure: 45–70 characters per line, target ~60 (`max-width: 62ch` on reading columns) — dyslexia- and ADHD-friendly.
- Left-aligned, ragged right. No justified text, no hyphenation.
- Paragraph spacing over indents; briefing paragraphs ≤ 3 sentences.
- User quotes always in `quote` style with a 2px amber left rule — the user's own words are sacred material and visibly distinct from AI text.

---

## 5. Voice & tone

### 5.1 Voice principles

1. **Second person, present, warm:** "You were halfway through the second chapter." Not "The user last edited…"
2. **Nothing is lost — ever.** The load-bearing promise; it appears verbatim on every welcome-back surface.
3. **Name the feeling, shrink the ask.** Empathy in ≤1 sentence, then one tiny concrete step. Never therapy-speak ("hold space", "journey").
4. **Honest about the machine.** "Ember filed this under *Etsy shop* — tap if that's wrong." Never pretend certainty.
5. **Small words, short sentences.** If a sentence needs a comma and two clauses, split it.
6. **Time is soft.** "a while ago", "back in March", "two months" — never precise elapsed-time counters ("47 days ago" is a guilt number).

### 5.2 Copy table (canonical examples)

| Moment | Say | Never say |
|---|---|---|
| Welcome back (weeks away) | "Welcome back. Nothing is lost. Three threads stayed warm while you were away — want the two-minute version?" | "You've been gone 47 days!" · "Your streak ended" · "You have 12 overdue items" |
| Welcome back (a few days) | "Morning. The novel is still warm — you stopped mid-scene, on purpose, right at the good part." | "Pick up where you failed to finish" · "Don't lose momentum!" |
| Filed toast (capture) | "Caught → filed to *Etsy shop*." (tap to correct) | "Task added to backlog" · "Inbox +1" · "Processed" |
| Loose Spark (low confidence) | "Caught. Ember is holding this one until you point it home." | "Uncategorized" · "Needs triage" · "Sort your inbox" |
| Doorway (normal day) | "One warm thread, one pebble, one old spark. Ninety seconds, then go live your day." | "Here's your task list" · "12 things need your attention" · "Be productive today!" |
| Doorway (low capacity) | "A full plan for a low-tank day: one two-minute pebble. That's the whole plan. It counts." | "Reduced plan" · "Light mode" · "Try to do at least one thing" · "Lazy day" |
| Pebble done | "Done. The thread's warmer. +40 ember minutes." | "Streak: 3 days!" · "Crushed it!" · "You're on fire! Keep grinding!" |
| Snooze | "Resting until next week. It'll keep — embers do." | "Postponed" · "Deferred" · "Overdue in 7 days" · "Are you sure? You'll fall behind" |
| Retire with honor | "The mural taught you color. Retiring it isn't quitting — it's finishing what it was for. Want to keep the closing note?" | "Deleted" · "Marked as failed" · "Abandoned" · "Gave up" |
| Unstick opener | "This one feels heavy. What's the flavor — boring, fuzzy, or scary?" | "Why are you procrastinating?" · "Break the task down and just start" · "No excuses" |
| Empty Shelf (first run) | "Your shelf is waiting for its first spark. Catch anything — a thought, a worry, a maybe. Ember will find it a home." | "No data yet" · "You have 0 projects" · "Get started by creating a task" |
| Error (AI down) | "Ember's memory-writing is napping. Everything you catch is safe — filing will catch up shortly." | "Error 503" · "Something went wrong" · "AI service unavailable" |
| Offline capture | "Caught and kept. It'll file itself when you're back online." | "Sync failed" · "Upload error" |
| Notification (self-silencing) | "These pings don't seem to be landing lately. Want me to go quiet for a while? Everything stays warm either way." | "You've ignored 5 notifications!" · "Last chance to keep your streak" |
| Billing pause (45 days idle) | "You've been away, so we paused your billing. Everything is safe and warm. Come back whenever." | "Your subscription is at risk" · "Don't lose your data — renew now!" |
| Trial ending | "Your trial ends Thursday. If Ember hasn't earned a place, no hard feelings — export takes one tap." | "Upgrade now before you lose access!" · "Last chance!" |

### 5.3 Banned vocabulary (hard list)

overdue · behind · late · missed · failed/failure · streak · backlog · inbox (use *Loose Sparks*) · task list / to-do · productivity/productive · grind · hustle · crush · lazy · procrastinate/procrastination (in user-facing copy) · discipline · "just" ("just start") · guilty/guilt-free · "you should" · deficit/disorder/symptom · urgent (except real calendar deadlines, phrased as "leave by 1:15").

Punctuation tone: at most one exclamation mark per screen, and almost never. No ALL-CAPS urgency. Emoji: none in UI chrome; at most one warm emoji in notification copy variants, never 🔥.

---

## 6. Iconography

- **Grid:** 24×24 px, 2px keylines. Live area 20×20.
- **Stroke:** 2px, round caps, round joins. Corner radius ≥ 2px inside glyphs — nothing sharp.
- **Style:** outlined at rest, **filled when active** (nav tabs, toggles). Fill uses the current text color; active accent fill is reserved for the Catch button alone.
- **Metaphors:** hearth-and-home (door, shelf, thread/loop, pebble, moon, hand). Banned glyphs: checkbox stacks, alarm bells, red-dot badges, flames-at-blaze-scale, trophies, lightning "productivity" bolts, brains.
- Base icon set: Lucide (2px, rounded) — customized glyphs must match its optical weight.
- Icon color: text color of context; never ember unless the icon *is* the primary action. Minimum rendered size 20px; touch targets around icons ≥ 44px.

---

## 7. Illustration & motif — the ember glow

The one proprietary visual: **a soft radial glow. Warmth = recency.**

- **Construction:** radial gradient, center Amber `#F5B942` → Ember `#E85D0F` at ~55% → transparent by 100%. On light surfaces render at 20–35% opacity; on dark, 30–45%. Always blurred/soft-edged (≥24px blur equivalent) — never a hard-edged circle.
- **Placement:** anchored to a corner or behind a key element (Catch button halo, warm-thread card corner, briefing header). One glow per surface, maximum two on marketing art.
- **Recency encoding:** touched today = fullest glow → this week = smaller/fainter → resting = no glow, dusty-blue cool tint (evening, not death) → retired = neutral + small sage mark.
- **Illustration style** (empty states, onboarding): flat warm shapes from the palette with grain/noise texture, soft geometry, no outlines; objects and interiors (desk, shelf, window at night, mug) — **no people's faces, no mascots, no anthropomorphized flames**. Scenes are quiet and slightly imperfect (a leaning book), never sterile.
- **Photography:** avoided in product; marketing may use warm-hour amber-graded photos of real, believably messy spaces. No stock "productive person smiling at laptop".

---

## 8. Motion

Motion is candlelight: it breathes, it never startles.

- **Durations:** micro-feedback (press, toggle) 120–150 ms · standard transitions (cards, toasts, sheets) **200 ms** · large surfaces & glow blooms (briefing reveal, welcome-back) **300 ms**. Nothing over 400 ms except the opt-in launch glow (600 ms, once).
- **Easings:** enter `cubic-bezier(0.22, 0.61, 0.36, 1)` (soft settle) · exit `cubic-bezier(0.4, 0.0, 0.7, 1)` · glow/opacity `ease-in-out`. **Never** bounce, spring-overshoot, elastic, shake, or wiggle — a shake on error is a scold.
- **Movement:** short fades + ≤12px translate. No parallax, no zoom-blur, no confetti. The deterministic micro-reward is a ≤300 ms glow bloom + haptic tick, rotated for novelty, and it fires before any network round-trip.
- **Reduced motion:** `prefers-reduced-motion: reduce` ⇒ all movement becomes opacity-only crossfades ≤150 ms; skeleton shimmer becomes a static two-tone block; the glow bloom becomes a simple fade. Feature-complete, never a degraded experience.
- **Loading:** skeletons appear only after 300 ms (the shell itself must render instantly); shimmer is a slow warm sweep (1.8 s linear), not a strobe.

---

## 9. Accessibility & ADHD-specific design laws

These are laws, not guidelines. A build that violates them fails review.

1. **One primary action per screen.** Exactly one ember-filled element. Everything else is secondary or quiet. (The four-button briefing row has one primary — *Do it now* — and three quiet siblings.)
2. **No red badges. No notification dots on internal navigation. No unread counts.** The app must never accumulate visible debt. The only numeric badge anywhere is the offline "safe, will file when online" queue chip — and it's amber and reassuring.
3. **Tap targets ≥ 44×44 px** (Catch button ≥ 64 px). Minimum 8 px between adjacent targets.
4. **Focus states:** 2px Ember outline + 2px surface-colored offset ring on every interactive element; visible in both themes; never removed. Full keyboard path on web (`C` = quick capture).
5. **Contrast:** all text meets AA per §3.3; ember-on-cream restricted to large/graphical uses.
6. **Reading:** ≤62ch measure, ≥1.45 line height, no justified text, no all-caps sentences, minimum 16px body.
7. **Bounded surfaces:** Doorway ≤ 3 items / ≤ 90 seconds; briefings show one tiny next step, never the wall of steps; Unstick hard-caps at 3 exchanges. Infinite scroll only on Shelf/Search (deliberate browse surfaces).
8. **Time blindness respect:** relative soft time ("back in March"); real deadlines rendered as lead-time ("leave by 1:15"), not countdown timers.
9. **No time-outs on input.** Capture drafts, half-finished corrections, and abandoned modals persist. Reopening mid-flow restores state — the app itself does Warm Starts.
10. **Motion & vestibular safety:** per §8; nothing flashes >3 Hz; no autoplaying video.
11. **Never-render rules** (from navigation map): no overdue counts, no streaks, no "you missed…", no guilt numerals.
12. **Screen reader parity:** glow/warmth states carry text equivalents ("warm — touched yesterday"); toasts are polite live regions; the Catch button is labeled "Catch a thought".

---

## 10. Do / Don't gallery (in words)

| # | Do | Don't |
|---|---|---|
| 1 | Doorway with one warm card, one ember button, cream space around it | A dashboard of 6 stat tiles, 3 charts, and a motivational quote |
| 2 | "Welcome back. Nothing is lost." in Fraunces over a soft glow | "47 days since your last visit" over a broken-streak graphic |
| 3 | Resting shelf on parchment with dusty-blue tint, labeled "Resting" | Grayed-out "inactive projects" list with warning triangles |
| 4 | Ink text on ember button ("Do it now") | White text on ember (fails AA), or three ember buttons on one screen |
| 5 | A retired Thread with a sage dot and its Closing Note | A trash can icon and a "deleted projects" graveyard |
| 6 | Notification: "The Etsy thread is warm if you want it. No rush." | Notification: "⏰ Don't forget! 5 tasks are waiting! Streak at risk!" |
| 7 | Night Catch: pure black, one dim amber hairline, silent | Night capture with white flash, keyboard clicks, confirmation chime |
| 8 | Error in terracotta: "That didn't save — your words are still here. Try again?" | Red banner: "ERROR: Sync failed (code 500)" |
| 9 | Empty state: warm illustration + one inviting line + one Catch button | Empty state: "No data. Create your first task to get started!" |
| 10 | Progress as "ember hours" quietly accumulating, visible on request | XP bars, levels, leaderboards, daily-goal rings |
| 11 | Spark quote in serif italic with amber rule: *"what if the logo was a match?"* | Paraphrasing the user's words into corporate summary bullets |
| 12 | One 300 ms glow bloom when a Pebble completes | Confetti cannon + badge modal + share prompt |

---

## 11. Asset checklist (before anything ships)

- [ ] Exactly one primary (ember) element per surface
- [ ] All text pairs from §3.3 or verified ≥ 4.5:1 (≥ 3:1 large/graphic)
- [ ] Zero banned words (§5.3); zero red; zero badges/dots/counts
- [ ] Serif only for warmth moments; body in system sans ≥ 16px, ≤ 62ch
- [ ] Motion ≤ 300 ms, soft easings, reduced-motion path present
- [ ] Wordmark per §2 (clearspace, spark position, min size)
- [ ] The 1 a.m. litmus test (§1.3) passes
