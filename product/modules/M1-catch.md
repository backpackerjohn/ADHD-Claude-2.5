# Module: M1 — Catch

> Conforms to `product/modules/_TEMPLATE.md`. Terminology, objects, and states per `product/product-brief.md` (source of truth). Origin: `contest/thesis-B.md` §4–5; binding synthesis + risks: `contest/winner.md`.

## 1. Purpose

Catch is Ember's zero-decision capture layer and the daily verb of the product (thesis: "the daily verb is depositing"). It exists because filing decisions kill capture ("Notion requires an organizational decision at the moment of every save… so the system gets silently abandoned" — [MindStash](https://www.mindstash.app/blogs/why-your-adhd-brain-hates-notion-and-what-actually-works-instead)) and because lost thoughts feel "like being betrayed by your own mind" ([InFocus First](https://infocusfirst.com/adhd-forgetfulness/)), with the working-memory mechanism confirmed vendor-independently ([CHADD](https://chadd.org/adhd-weekly/adhd-can-trip-up-memories/)). Catch turns any thought into a **Spark** in ≤2 seconds with zero organizational decisions, auto-files it into a **Thread**, and feeds the memory that makes M3 Warm Start possible. Founder mandate #1: organize thoughts. Owned risks: R1 (capture-lane gravity vs Apple Notes), R2 (instant visible filing collapses the action→reward delay — [JAD delay-discounting meta-analysis](https://journals.sagepub.com/doi/10.1177/1087054718772138)), R3 (misclassification vs "zero-decision" trust).

## 2. User goals

- I want to get a thought out of my head in two seconds, so that my brain can stop rehearsing it.
- I want to never choose a folder, tag, or title, so that capture never becomes a decision I avoid.
- I want to see my thought land somewhere real ("filed to *Etsy shop*"), so that I trust it isn't lost.
- I want to capture at 1am without light, sound, or a feed, so that I can go back to sleep.
- I want to fix a misfiled thought with one drag, so that one mistake doesn't break my trust in filing.
- I want to import my old Apple Notes and Notion graveyard, so that my past becomes memory instead of guilt.
- I want capture to work with no signal, so that I never have to think about connectivity.

## 3. Objects

| Object | Role in M1 | Fields owned by M1 |
|---|---|---|
| **Spark** (owned) | The unit of capture | `id`, `body_text`, `audio_ref` (nullable), `transcript_source` (on-device \| server \| none), `captured_at`, `capture_surface` (widget \| share_sheet \| in_app \| night \| web \| import), `filed_thread_id` (nullable), `confidence` (0–1), `filing_state` (unfiled/Loose · filed · resurfaced · archived), `correction_history[]`, `import_batch_id` (nullable), `dedupe_hash`, `sync_rev` |
| **ImportBatch** (owned, M1-internal record) | One graveyard import run | `source` (apple_notes \| notion_export), `total`, `processed`, `skipped_dupes`, `failed[]`, `token_spend`, `status` (running · paused_cap · complete · partial) |
| **Thread** (touched) | Filing target; M1 may create a *proposed* Thread from a high-confidence new-topic Spark | none (owned by M2) |
| **Digest** (touched) | Every filed Spark is queued into the nightly digest update | none (owned by M2/AI layer) |

## 4. Lifecycle

**Spark:** created by any capture surface or import → `unfiled (Loose)` or `filed` (see §8 gate) → may become `resurfaced` (by M5 Doorway) or `archived` (user action or Thread retirement). Transitions: Loose→filed (auto after classifier retry, or user one-drag); filed→filed' (one-drag correction, logged to `correction_history`); any→archived (user; undoable 30 days, then purge on request only — default retention is forever, memory is the product). Deletion: explicit user delete = hard delete of body + audio within 24h. Audio: raw audio kept 7 days after successful transcription then deleted; transcript is permanent.

**ImportBatch:** created by web-app import wizard → `running` → `paused_cap` (hit per-batch AI spend cap; resumable) → `complete` or `partial` (some items failed). Retained as a receipt the user can reopen.

## 5. Actions

| Action | Trigger surface | Input | Effect | Undo |
|---|---|---|---|---|
| Capture text | Lockscreen widget · in-app big button (home, thumb-zone, ≥64pt) · web quick-capture (global `C` hotkey) | typed text | Spark created locally, instant feedback (§8 deterministic layer), enqueue for filing | Delete within toast (5s "Undo") |
| Capture voice | Same surfaces, hold-or-tap mic | audio | Spark created with `audio_ref`; on-device transcription starts immediately | Same |
| Share into Ember | OS share sheet (iOS/Android) | text/URL/image-with-caption | Spark created; source URL kept in body | Same |
| Night catch | Widget long-press or in-app moon toggle (auto-offered 23:00–06:00) | text/voice | Black screen, no sounds, no toast, no filing shown; Spark routes to tomorrow's Doorway (M5) | Silent; undo available next morning |
| One-drag correction | Spark card (Loose Sparks tray, Thread view, toast) | drag Spark onto a Thread on the Shelf strip, or long-press → "Move to…" | Refile; emits training signal (§8); toast "Moved to *X* — I'll learn from that" | Drag back / Undo in toast |
| Confirm/adopt proposed Thread | Loose Sparks tray card | tap "Yes, new thread" / "No, belongs in…" | Creates Thread (M2) or refiles | Undo in toast |
| Start graveyard import | Web app → Settings → "Import the wreckage" | Apple Notes export (.zip of .txt/.html) or Notion export (.zip md/csv) | Creates ImportBatch; background processing (§8) | Cancel keeps already-imported Sparks; batch deletable in one tap ("Remove this import") |
| Archive Spark | Swipe on Spark card | — | `archived` | Restore from Thread's archive list, 30 days highlighted |

No watch app, no browser clipper in v1 (R5: month-one scope is mobile app + widget + web app). Web quick-capture is a page inside the web app, not an extension.

## 6. States

- **Empty / first-run:** big button + one line: "Say anything. Ember will file it." First capture triggers a one-time 3-second explainer of the toast ("It went to a thread — watch"). No Thread exists yet → first 1–3 Sparks create proposed Threads; cold-start is expected and fine (capture relief is day-one value).
- **Ideal:** capture → <300ms deterministic feedback → within ~2–5s toast "Caught → filed to *Etsy shop*" with the Thread's ember glyph warming.
- **Loading / filing-pending:** Spark shows a soft "settling…" shimmer, never a spinner-blocked UI; user can keep capturing.
- **Offline:** full capture works; Sparks enter the local queue with state chip "safe on your phone — will file when back online." Never an error tone. On reconnect, queue files in order; one summary toast ("6 thoughts filed while you were away"), not six toasts.
- **Transcription failure:** on-device ASR fails or confidence garbage → Spark keeps audio, card shows play button + "Couldn't hear this one — tap to retry or type it." One tap retries via server ASR (consented in onboarding, skippable). Never silently dropped.
- **Misfile-correction:** user drags Spark from wrong Thread; both Threads flash origin→destination; copy: "Got it — *shop logo* things go to *Etsy shop*." No shame language, no "error."
- **Loose Sparks tray:** visible, warm ("Ember is holding these — 4 sparks"), badge is an ember count, never red, never named "inbox." Tray capped visually at top 20 + "older sparks" fold.
- **Import-in-progress:** progress card on web + mobile ("312 of 4,000 notes read · 61 duplicates skipped"), pausable; if the cost cap pauses it: "Paused to keep things affordable — resumes tonight."
- **Returning after weeks:** Catch itself never scolds. Widget and big button unchanged; Loose Sparks tray shows a gentle merge offer ("A few sparks piled up — want me to refile them in one go?") which batch-files everything above threshold. Welcome-back framing belongs to M5/M3, not here.

## 7. Workflows

**W1 — Happy path (voice, widget)** [wireframes WF-M1-01…04]: lockscreen widget tap → mic screen (WF-M1-01) → speak, release → deterministic catch animation <300ms (WF-M1-02) → on-device transcript → classifier files → toast "Caught → filed to *Etsy shop*" (WF-M1-03) → Spark visible atop Thread (WF-M1-04). Total user attention: ~2s.

**W2 — Failure: low-confidence filing** [WF-M1-05]: classifier returns confidence < threshold → Spark lands in Loose Sparks tray with top-2 Thread guesses as one-tap chips + "new thread?" chip → user taps or drags → correction logged as training signal. Tray item never expires and is resurfaced at most once by M5.

**W3 — Failure: offline + ASR failure at night** [WF-M1-06]: 1am, airplane mode, night catch → audio stored, no transcription attempted (night mode defers all processing) → morning: device back online, on-device ASR runs; if it fails, Spark appears in tomorrow's Doorway as playable audio card "from last night — tap to hear yourself." Nothing lost, nothing demanded at 1am.

**W4 — Graveyard import** [WF-M1-07…09]: web app wizard → upload export → parse locally in browser where possible → dedupe pass (§9) → batched classification into Threads/Loose → completion screen: "1,240 notes are now memory. 3 threads look alive — want a Warm Start on any of them?" (hand-off to M3).

## 8. AI behavior

- **Trigger:** every non-night Spark on creation (night Sparks: next morning, batched). **Input:** Spark text + candidate list of the user's Thread names with one-line Digest summaries (prompt-cached stable prefix). **Model:** Haiku-tier (workhorse per brief; ≈$1.31/mo at 25 captures/day, thesis-B §8).
- **Output contract (JSON):** `{thread_id | new_thread_proposal{name} | loose, confidence: 0–1, alt_thread_ids[≤2]}`. Malformed output ⇒ treated as `loose`.
- **Confidence gate (R3 mitigation):** confidence ≥ 0.8 → auto-file with visible toast naming the Thread (filing is always announced, never silent). 0.5–0.8 → file but toast carries an inline "not right? drag it" affordance. < 0.5 → Loose Sparks tray, **never a hidden inbox** ("out of sight, out of mind is a neurological reality" — [Medium/Brunell](https://raymond-brunell.medium.com/i-deleted-47-productivity-apps-in-30-days-heres-what-actually-worked-for-my-adhd-brain-52c292c6ba6b)). Week-one conservative mode: thresholds shifted up 0.1 while corrections < 10, because early misfiles are the trust-killer.
- **Corrections as training:** every one-drag correction appends `(spark_text → correct_thread)` to a per-user few-shot exemplar block (last 30 corrections, cached) injected into the classifier prompt. No fine-tuning in v1.
- **Import classification:** same classifier, Batch API (50% off), hard cap **$1.50 of model spend per ImportBatch** (cost-capped per winner.md #16); past cap → remaining items land pre-deduped in an "Imported, unsorted" holding shelf section, classified opportunistically by the nightly batch over following nights.
- **Fallback when AI unavailable:** Sparks queue as Loose with "settling later" chip; capture is never blocked by the API.
- **Deliberately NOT AI (deterministic, <300ms):** capture recording, the catch animation + haptic + one of 12 rotating micro-reward variants (novelty rotation, anti-habituation), offline queueing, on-device transcription (platform speech APIs / Whisper-class on-device, default; server ASR only as consented fallback), dedupe hashing, all night-mode behavior, toast timing. The reward layer must never wait on an API round-trip (delay-discounting compliance, winner.md steal-list #2).

## 9. Scale

- **10k+ Sparks (heavy importer or year-two user):** Spark list virtualized; Thread views paginate 50 Sparks/page with time-bucket jump ("last spring"). Loose Sparks tray shows 20 + fold regardless of backlog. Classifier candidate list caps at 60 Threads: warm + recently-touched first, resting Threads represented by cluster summaries — filing latency must not grow with corpus.
- **Import at 100× (4,000-note Apple Notes wrecks — cf. [Medium](https://medium.com/macoclock/how-i-decluttered-4-000-apple-notes-in-15-minutes-a-week-7417819ba159)):** dedupe pre-pass is local and deterministic — normalized-text hash for exact dupes, MinHash near-dupe within batch and against existing corpus; dupes skipped and counted, never double-imported on re-run (idempotent by `dedupe_hash` + `import_batch_id`).
- **Performance budgets:** capture UI interactive < 200ms cold from widget; feedback < 300ms always; auto-file round trip target < 5s p90 (async, non-blocking); search over Sparks is M2's hybrid search — M1 only guarantees embeddings are queued on filing.

## 10. Errors

- **Sync conflicts (phone vs web):** Sparks are append-only, so creation never conflicts. Conflicting *filing* states (e.g., corrected on web while offline phone auto-filed) resolve last-writer-wins by `sync_rev` with one exception: a **user correction always beats a classifier decision** regardless of timestamp. Conflict is silent; no dialog ever asks the user to merge a thought.
- **Partial import:** unreadable/failed items listed on the ImportBatch receipt with "retry these" one-tap; batch marked `partial`, re-running is idempotent. Never fail the whole batch for bad items.
- **ASR failure:** see §6/§7-W3 — audio always retained until a transcript exists; retry ladder: on-device → server (consented) → "type it" with audio playback.
- **AI outage:** everything routes to Loose with honest chip copy; recovery job files the backlog and emits one summary toast.
- **User mistakes:** every destructive/filing action has toast-undo (5s) plus 30-day restore for archive; deleted-by-mistake import removable/re-runnable as a unit.
- **Duplicate capture** (double-tap widget): identical Spark within 10s collapses into one, silently.

## 11. Permissions

Private by default; no sharing surfaces in M1 (or v1 generally). ADHD is health-adjacent data: Spark contents encrypted in transit and at rest; on-device transcription default means raw voice never leaves the phone unless the user opts into server ASR fallback (explicit onboarding consent, revocable in settings). Import files processed for the user's account only, purged from server storage after batch completion; audio purged 7 days post-transcript. No Spark content in analytics events (event names + counts only). Model calls run with no-training/no-retention API configuration per privacy-first commitment (brief, cross-cutting systems).

## 12. Dependencies

- **Requires:** M2 Threads & Shelf (filing targets, Shelf strip for one-drag, archive surfaces); AI layer per `ai-spec.md` (Haiku classifier, Batch import, embeddings); platform: widget APIs, share-sheet extensions, on-device speech; local queue store.
- **Emits:** `spark.captured` (M5 counts, ember-hours layer), `spark.filed` / `spark.corrected` (M2 Thread story; nightly Digest queue; classifier exemplars), `spark.loose` (M5 may resurface one), `import.completed` (M3 may offer Warm Starts on revived Threads), `night.captured` (routes to tomorrow's Doorway, M5).
- **Consumes:** `thread.created/renamed/retired` (candidate list maintenance, M2), Digest one-liners (classifier context), M5's morning window (night-Spark release).
