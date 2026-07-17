# Ember — Navigation, Pages, Flows & States Map

Terminology and objects per `product-brief.md`. Wireframe IDs (`W-…`) defined here are canonical; wireframes and prototype must use them.

## Global navigation

**Mobile (capture-first):** bottom bar with 3 destinations + 1 action:
`Doorway` (home) · `Shelf` · `Search` · **[ + Catch ]** (large center action button — always one tap from anywhere). Night mode auto-themes the Catch surface 22:00–06:00. Lockscreen widget and share sheet enter directly into Catch.

**Web (memory-first):** left sidebar: `Doorway` · `Shelf` · `Search` · `Loose Sparks` · `Settings`; persistent quick-capture field pinned top (keyboard shortcut `C`). Thread opens as main-pane page.

## Page inventory (canonical wireframe IDs)

| ID | Page/Surface | Platform | Module |
|---|---|---|---|
| W-01 | Doorway (daily card) | both | M5 |
| W-01b | Doorway — welcome-back variant | both | M5 |
| W-01c | Doorway — low-spoon (capacity dial) variant | both | M5 |
| W-02 | Catch overlay (text/voice) | both | M1 |
| W-02b | Catch — night mode | mobile | M1 |
| W-02c | Catch — filed toast (+ Loose fallback) | both | M1 |
| W-03 | Shelf (Warm row · Resting shelf · Finished & Retired gallery) | both | M2 |
| W-04 | Thread story (chronological view) | both | M2 |
| W-04b | Thread story — with inline Warm Start briefing at top | both | M3 |
| W-05 | Warm Start briefing (full, ≥14 days) | both | M3 |
| W-05b | Briefing — whisper/brief tiers | both | M3 |
| W-06 | Unstick interaction (3-exchange, bounded) | both | M3 |
| W-07 | Do-it-now focus surface (Pebble + timer + done) | both | M3 |
| W-08 | Retire with honor flow (confirm → Closing Note) | both | M3 |
| W-09 | Arc proposal review (edit milestones before accept) | both | M4 |
| W-10 | Arc view (milestones collapsed; next Pebble hero) | both | M4 |
| W-10b | Arc — re-planned diff ("the plan bent, it didn't break") | both | M4 |
| W-10c | Arc — deadline-missed amnesty fork (re-aim · shrink · retire) | both | M4 |
| W-11 | Search & recall (hybrid; temporal filters) | both | M2 |
| W-12 | Loose Sparks tray (one-drag filing) | both | M1 |
| W-13 | Import wizard (Apple Notes/Notion; progress) | web | M1 |
| W-13b | Import summary ("1,204 sparks joined 9 threads") + guided Loose triage | web | M1 |
| W-14 | Settings: account, notifications, capacity default, privacy/export | both | — |
| W-15 | Billing (honest: price, two-tap cancel, pause state) | both | — |
| W-16 | Onboarding (first Catch → first Thread → Doorway preview + notification-window picker + server-ASR consent, skippable) | both | — |
| W-17 | Empty/first-run states gallery (Shelf, Doorway, Search) | both | — |

Additional canonical states: W-01 includes a **night-ASR-retry card** (playable audio from failed night transcription) and the phase-2 **felt-time line** ("leave by 1:15"); W-01 has an optional **evening close** variant **W-01d** (silent rollover, no debt); W-03 includes an **"Imported, unsorted"** holding section state; W-10 includes a **deadline-approaching** state.

## Modals & overlays

| ID | Overlay | Notes |
|---|---|---|
| O-01 | Filed toast ("Caught → filed to *Etsy shop*", tap to correct) | <300ms deterministic shell; name fills async |
| O-02 | Misfile correction sheet (thread picker, one drag/tap) | teaches classifier |
| O-03 | Snooze picker (tomorrow · next week · when I touch it) | no dates guilt |
| O-04 | Shrink sheet (Arc scope renegotiation, 3 preset shrinks) | M4 |
| O-05 | Closing Note preview (editable before saving) | M3 |
| O-06 | Capacity dial (3-position control on Doorway) | M5 |
| O-07 | Notification check-in ("These pings aren't landing — pause them?") | anti-habituation self-silencing |
| O-08 | Merge threads confirm (incl. arc-survivor conflict resolution: which Arc survives, loser pauses) | bulk action |
| O-09 | Delete/export data confirm (typed confirm for delete) | privacy |
| O-10 | Crisis-resource sheet (deterministic static response; detection = local patterns + Haiku assist) | never AI-improvised |
| O-11 | Digest viewer panel ("What Ember knows about this thread" + "Correct something") | R3 trust mitigation |

## Core flows (happy path → failure paths)

**F1 Capture:** anywhere → [+] → speak/type → close. O-01 confirms filing; low confidence → W-12 Loose Sparks (visible, never hidden). *Failures:* offline → queued badge "safe, will file when online"; ASR fail → raw audio kept + retry chip.

**F2 Morning:** notification (varied copy, capped) → W-01 Doorway → tap Pebble → **W-07 do-it-now directly**, with the thread's whisper line embedded at the top of the focus surface (canonical ruling D-016: one tap to action; whisper context preserved without a detour through the thread). → done → deterministic reward + tomorrow's card seeds. *Failure:* card skipped → nothing; no rollover debt.

**F3 Re-entry (crown jewel):** open W-03 Shelf → tap resting thread → W-05 briefing → four buttons → `Do it now` → W-07. *Failures:* insufficient memory → honest fallback (thread story + last Breadcrumb); AI down → W-04 story view with banner "memory view only right now".

**F4 Big thing → Arc:** W-04 thread → "make this an arc" → W-09 proposal review (user edits) → accept → W-10 arc live; Doorway carries ≤1 arc Pebble/day. *Failure:* decomposition weak → user edits milestones inline or regenerates with a hint.

**F5 Unstick:** W-07 or any Pebble → "I'm stuck" → W-06 3 exchanges max → 2-minute move or "take the break, the Pebble will wait". Hard cap enforced.

**F6 Lapse & return:** ≥7 quiet days → no guilt pings (self-silenced after ignores) → user returns → W-01b welcome-back ("Nothing is lost") → 45+ days → billing auto-paused (W-15 shows paused state + resume).

**F7 Retire:** W-05 or W-04 → Retire with honor → O-05 Closing Note preview → Finished & Retired gallery (W-03). Recovery: reopen from gallery → thread rekindles (warm) with a "back from retirement" whisper briefing.

**F8 Find it:** W-11 search: "that idea about the logo, sometime in spring" → results grouped by thread with spark quotes → tap → W-04 scrolled to spark. Filters: type, thread state, time range. Bulk: move/merge from results.

**F9 Import:** W-13 wizard → upload export → cost-capped batch filing → summary ("1,204 sparks joined 9 threads; 312 in Loose Sparks") → guided triage of top Loose clusters.

**F10 Cancel/pause:** W-15 → two taps to cancel, no retention maze; idle 45 days → auto-pause email + in-app note; resume in one tap.

## State matrix (minimum states every wireframe must show)

Every page: `empty/first-run`, `ideal`, `loading (skeleton, <300ms shell)`, `offline`, `AI-degraded`, `returning-after-weeks`. Never-render rules: no red badges, no overdue counts, no streaks, no "you missed…".
