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

**Spark** (states per brief object model: unfiled (Loose) → filed → resurfaced / archived):
- **Created by:** any §5 capture surface, or an ImportBatch. Creation is local-first: the Spark row is committed on-device before any network or AI work.
- **unfiled (Loose) → filed:** classifier auto-file above threshold (§8), a later batch retry, or a user one-drag. There is no other exit from Loose — Loose Sparks never expire and are never auto-archived.
- **filed → filed′ (refiled):** one-drag correction; old and new Thread ids appended to `correction_history` (this log is the classifier's training signal).
- **filed → resurfaced:** set by M5 when the Doorway shows the Spark ("you had a good idea about the shop logo on Tuesday"); M1 only stores the flag.
- **any → archived:** user swipe, or side-effect of Thread retirement (M2). Undo highlighted for 30 days; restorable forever after that.
- **Deletion:** only by explicit user delete — hard-deletes body, transcript, and audio within 24h. Default retention is forever: memory is the product; there is no auto-purge.
- **Audio:** default retention — the transcript is permanent; raw audio is deleted 7 days after a successful transcription. A user setting **"Keep original audio"** (off by default) instead retains the audio in cold storage. Audio with no transcript yet is kept indefinitely (see §10 ASR failure).

**ImportBatch:**
- **Created by:** the web-app import wizard (§5). One batch per uploaded export file.
- **running → paused_cap:** per-batch AI spend cap hit (§8); resumes automatically via the nightly batch job, or manually.
- **running → complete | partial:** `partial` when some items failed to parse (§10); the receipt lists them with one-tap retry.
- **Retention:** kept permanently as a receipt; deletable as a unit, which archives (not deletes) its Sparks unless the user confirms full removal.

## 5. Actions

| Action | Trigger surface | Input | Effect | Undo |
|---|---|---|---|---|
| Capture text | Lockscreen widget · in-app big button (home, thumb-zone, ≥64pt) · web quick-capture (global `C` hotkey) | typed text | Spark created locally, instant feedback (§8 deterministic layer), enqueue for filing | Delete within toast (5s "Undo") |
| Capture voice | Same surfaces, hold-or-tap mic | audio | Spark created with `audio_ref`; on-device transcription starts immediately | Same |
| Share into Ember | OS share sheet (iOS/Android) | text/URL/image-with-caption | Spark created; source URL kept in body | Same |
| Night catch | Widget long-press or in-app moon toggle (auto-offered 22:00–06:00) | text/voice | Black screen, no sounds, no toast, no filing shown; Spark routes to tomorrow's Doorway (M5) | Silent; undo available next morning |
| One-drag correction | Spark card (Loose Sparks tray, Thread view, toast) | drag Spark onto a Thread on the Shelf strip, or long-press → "Move to…" | Refile; emits training signal (§8); toast "Moved to *X* — I'll learn from that" | Drag back / Undo in toast |
| Confirm/adopt proposed Thread | Loose Sparks tray card | tap "Yes, new thread" / "No, belongs in…" | Creates Thread (M2) or refiles | Undo in toast |
| Start graveyard import | Web app → Settings → "Import the wreckage" | Apple Notes export (.zip of .txt/.html) or Notion export (.zip md/csv) | Creates ImportBatch; background processing (§8) | Cancel keeps already-imported Sparks; batch deletable in one tap ("Remove this import") |
| Archive Spark | Swipe on Spark card | — | `archived` | Restore from Thread's archive list, 30 days highlighted |

No watch app, no browser clipper in v1 (R5: month-one scope is mobile app + widget + web app). Web quick-capture is a page inside the web app, not an extension.

## 6. States

- **Empty / first-run:** big button + one line: "Say anything. Ember will file it." First capture triggers a one-time 3-second explainer of the toast ("It went to a thread — watch"). No Thread exists yet → first 1–3 Sparks create proposed Threads; cold-start is expected and fine (capture relief is day-one value; no setup, no folder-making onboarding).
- **Ideal:** capture → <300ms deterministic feedback → within ~2–5s toast "Caught → filed to *Etsy shop*" with the Thread's ember glyph warming.
- **Loading / filing-pending:** Spark shows a soft "settling…" shimmer, never a spinner-blocked UI; user can keep capturing; the queue never blocks the mic.
- **Offline:** full capture works; Sparks enter the local queue with state chip "safe on your phone — will file when back online." Never an error tone, never a retry demand. On reconnect, queue files in order; one summary toast ("6 thoughts filed while you were away"), not six toasts.
- **Transcription failure:** on-device ASR fails or returns garbage-confidence → Spark keeps audio, card shows play button + "Couldn't hear this one — tap to retry or type it." One tap retries via server ASR (consented in onboarding, skippable). A failed-ASR Spark still counts as caught; it is never silently dropped.
- **Misfile-correction (O-02):** user drags Spark out of the wrong Thread; origin→destination flash on the Shelf strip; copy: "Got it — *shop logo* things go to *Etsy shop*." No shame language, no "error," no confirmation dialog.
- **Loose Sparks tray:** always visible from home, warm framing ("Ember is holding these — 4 sparks"), badge is an ember count, never red, never named "inbox" (terminology rule). Renders the top 20 + a lazy-loaded "older sparks" fold; Loose Sparks never expire and are never auto-archived.
- **Import-in-progress:** progress card on web + mobile ("312 of 4,000 notes read · 61 duplicates skipped"), pausable and resumable; if the cost cap pauses it: "Paused to keep things affordable — the rest files itself tonight."
- **Returning after weeks:** Catch itself never scolds and never changes shape — widget and big button identical to day one (muscle-memory preservation, R1). If Loose Sparks accumulated, one gentle merge offer: "A few sparks piled up — want me to refile them in one go?" → batch-files everything above threshold in one tap. Welcome-back narrative belongs to M5/M3, not here.

## 7. Workflows

**W1 — Happy path (voice, widget)** [wireframes W-02 → O-01 → W-02c]
1. Lockscreen widget tap → mic screen (W-02 Catch overlay), already recording; no login wall, no navigation.
2. Speak, release (or auto-stop on 1.5s silence). Deterministic catch animation + haptic fires <300ms (W-02); phone can be pocketed now — everything after is async.
3. On-device transcription → Spark queued → Haiku classifier files it.
4. Toast (if app/lockscreen visible): "Caught → filed to *Etsy shop*" with Thread ember glyph warming (O-01 / W-02c filed-toast state); Spark sits atop the Thread story (W-04, M2).
5. Total demanded attention: ~2 seconds; total decisions: zero.

**W2 — Failure path: low-confidence filing** [W-12]
1. Classifier confidence < 0.55 → Spark lands in Loose Sparks tray: "Ember is holding this one."
2. Card shows top-2 Thread guesses as one-tap chips + "new thread?" chip; or user drags it onto the Shelf strip.
3. Tap/drag files it and logs the correction as a training exemplar (§8). Tray items never expire, never turn red, and are resurfaced by M5 at most once each.

**W3 — Failure path: offline + ASR failure at night** [W-02b → W-01 night-ASR-retry card state]
1. 1am, airplane mode, night catch: black screen, audio stored locally, zero processing attempted (night mode defers everything — no light, no toast, no result to look at).
2. Morning, back online: on-device ASR runs in the batch release.
3. If ASR fails, the Spark surfaces in today's Doorway as a playable audio card: "from last night — tap to hear yourself," with retry-via-server and type-it options. Nothing lost, nothing demanded at 1am.

**W4 — Graveyard import** [W-13 → W-13b]
1. Web app → Settings → "Import the wreckage" (W-13 import wizard): pick Apple Notes export or Notion export zip.
2. Parse + dedupe locally in the browser where feasible (§9); upload survivors; ImportBatch starts (W-13 progress-card state, pausable, cost-capped).
3. Batched classification files notes into existing/proposed Threads; leftovers land in "Imported, unsorted."
4. Completion (W-13b import summary): "1,240 notes are now memory. 61 duplicates skipped. 3 threads look alive — want a Warm Start on any of them?" (hand-off to M3). The framing is memory-not-guilt, per winner.md #16.

## 8. AI behavior

- **Trigger:** every non-night Spark on creation (night Sparks: next morning, batched). **Model:** Haiku-tier (workhorse per brief; ≈$1.31/mo at 25 captures/day, thesis-B §8).
- **Prompt contract (input):** cached stable prefix = system instructions + the user's Thread candidate list (name + one-line Digest summary each) + last-30 correction exemplars; variable suffix = the Spark text and `captured_at`. One call per Spark; imports batch ~20 Sparks per call.
- **Output contract (JSON, strict):** `{"decision": "file" | "propose_new_thread" | "loose", "thread_id": "...", "new_thread_name": "...", "confidence": 0.0–1.0, "alt_thread_ids": [≤2]}`. Malformed or non-JSON output ⇒ treated as `loose` (fail safe, never fail wrong).
- **Confidence gate (R3 mitigation):** confidence ≥ 0.8 → auto-file with visible toast naming the Thread (filing is always announced, never silent). 0.5–0.8 → file but toast carries an inline "not right? drag it" affordance. < 0.5 → Loose Sparks tray, **never a hidden inbox** ("out of sight, out of mind is a neurological reality" — [Medium/Brunell](https://raymond-brunell.medium.com/i-deleted-47-productivity-apps-in-30-days-heres-what-actually-worked-for-my-adhd-brain-52c292c6ba6b)). Week-one conservative mode: thresholds shifted up 0.1 while corrections < 10, because early misfiles are the trust-killer.
- **Corrections as training:** every one-drag correction appends `(spark_text → correct_thread)` to a per-user few-shot exemplar block (last 30 corrections, cached) injected into the classifier prompt. No fine-tuning in v1.
- **Import classification:** same classifier, Batch API (50% off), hard cap **$1.50 of model spend per ImportBatch** (cost-capped per winner.md #16); past cap → remaining items land pre-deduped in an "Imported, unsorted" holding section on the Shelf, classified opportunistically by the nightly batch over following nights until drained.
- **Guardrails:** classifier may never delete, merge, or rename Threads — it only files and proposes; new-thread proposals require a user tap to become real Threads (except during first-run cold start, where the first auto-created Threads are labeled "Ember started this thread — rename or merge anytime"); filing is always visibly announced (toast or tray), never silent (R3: silent misfiles are how "zero-decision" trust dies); Spark text is passed verbatim, never rewritten by the model.
- **Fallback when AI unavailable:** Sparks queue as Loose with "settling later" chip; capture is never blocked by the API (see §10).
- **Deliberately NOT AI (deterministic, <300ms feedback layer):** capture recording and storage; the catch animation + haptic + one of 12 rotating micro-reward variants (novelty rotation, anti-habituation — winner.md steal-list #2: rewards stay out of the API round-trip, delay-discounting compliance); offline queueing and sync; on-device transcription (platform speech APIs / Whisper-class on-device, default; server ASR only as consented fallback); dedupe hashing; all night-mode behavior; toast timing and undo windows. If every model call failed forever, Catch would still capture, queue, and display — only auto-filing degrades.

## 9. Scale

- **10× (≈2,500 Sparks, 30 Threads — an engaged first year):** no behavior change; classifier candidate list still fits one cached prefix; Thread stories paginate at 50 Sparks/page.
- **100× / 10k+ Sparks (heavy importer or multi-year user):** all Spark lists virtualized; Thread views keep 50/page with time-bucket jump ("last spring"); Loose Sparks tray still renders 20 + fold regardless of pile-up size. Classifier candidate list caps at 60 Threads — warm + recently-touched first, resting Threads represented by cluster one-liners — so filing latency and cost never grow with corpus size. Old filed Sparks are cold storage: hydrated on Thread open, not resident.
- **Import at scale (4,000-note Apple Notes wrecks — cf. [Medium](https://medium.com/macoclock/how-i-decluttered-4-000-apple-notes-in-15-minutes-a-week-7417819ba159)):** dedupe pre-pass is local and deterministic — normalized-text hash for exact dupes, MinHash near-dupe within the batch and against the existing corpus; dupes skipped and counted, never double-imported on re-run (idempotent by `dedupe_hash` + `import_batch_id`). Classification runs in nightly Batch chunks; a 4,000-note import is allowed to take 2–3 nights under the cost cap and says so.
- **Performance budgets:** capture surface interactive < 200ms cold from widget; deterministic feedback < 300ms always, on-device, unconditionally; auto-file round trip < 5s p90 (async, never blocking the next capture); on-device transcription start < 1s after recording ends. Search over Sparks belongs to M2's hybrid search — M1's only obligation is that every filed Spark has its embedding queued on filing.

## 10. Errors

- **Sync conflicts (phone vs web):** Sparks are append-only, so creation never conflicts — a Spark captured on both surfaces is two Sparks (then deduped per below). Conflicting *filing* states (e.g., corrected on web while the offline phone's queued auto-file lands later) resolve last-writer-wins by `sync_rev`, with one hard exception: a **user correction always beats a classifier decision**, regardless of timestamp. Resolution is silent; no dialog ever asks the user to merge a thought.
- **Partial import:** unreadable/corrupt/oversize items are skipped and listed on the ImportBatch receipt with a one-tap "retry these"; batch marked `partial`, never failed wholesale. Re-running the same export is idempotent (`dedupe_hash`): already-imported notes are skipped and counted, not duplicated.
- **ASR failure:** see §6 and §7-W3 — audio is always retained until a transcript exists; retry ladder: on-device retry → server ASR (consented) → "type it" with audio playback inline.
- **AI outage / rate limit:** capture unaffected (deterministic path); unfiled Sparks route to Loose with honest chip copy ("settling later"); a recovery job drains the queue when the API returns and emits one summary toast, not a storm.
- **Partial write (Spark saved, embedding/digest enqueue lost):** outbox pattern — side-effects are re-derived from the Spark row by a reconciliation sweep; the Spark itself is the only write that must succeed at capture time.
- **User mistakes:** every destructive or filing action has toast-undo (5s); archive has 30-day highlighted restore; an entire ImportBatch is removable or re-runnable as a unit ("Remove this import").
- **Duplicate capture** (double-tap widget, share-sheet double-fire): identical body within 10s collapses into one Spark, silently.

## 11. Permissions

- **Visibility:** all Sparks are private to the account owner. No sharing, collaboration, or public surfaces in M1 (or anywhere in v1).
- **Health-adjacent data posture:** ADHD-related content is treated as sensitive by default — Spark bodies, transcripts, and audio encrypted in transit and at rest; lockscreen widget shows the capture field only, never recent Spark contents.
- **Voice:** on-device transcription is the default, so raw audio never leaves the phone unless the user opts into the server ASR fallback — explicit consent screen during onboarding (skippable), revocable in Settings at any time; the retry ladder simply ends at "type it" if declined.
- **Import:** uploaded export files are processed for this account only and purged from server storage when the ImportBatch completes or is cancelled.
- **Analytics:** event names, counts, latencies, and confidence buckets only — never Spark content, Thread names, or transcripts.
- **Model calls:** no-training / zero-retention API configuration, per the brief's privacy-first cross-cutting commitment. Correction exemplars used in prompts stay inside the user's own account context.
- **OS permissions requested:** microphone (first voice capture, not at install), speech recognition (on-device), notifications (deferred to M5's flow — Catch itself never asks).

## 12. Dependencies

- **Requires:**
  - **M2 Threads & Shelf** — filing targets, Thread creation from proposals, the Shelf strip used by one-drag correction, archive/restore surfaces, hybrid search over filed Sparks.
  - **AI layer** (`product/ai-spec.md`) — Haiku-tier classifier endpoint, nightly Batch runner (imports, night Sparks, Loose retries, digest queue), embedding pipeline, per-user spend metering (feeds the $5 killswitch).
  - **Platform** — lockscreen widget APIs, share-sheet extensions (iOS/Android), on-device speech frameworks, local durable queue store (offline capture), background sync.
- **Emits:** `spark.captured` (M5 counters, ember-hours cross-cutting layer), `spark.filed` and `spark.corrected` (M2 Thread story; nightly Digest queue; classifier exemplar store), `spark.loose` (M5 may resurface at most one per Doorway), `night.captured` (held for tomorrow's Doorway, M5), `import.completed` (M3 may offer Warm Starts on revived Threads; M2 refreshes Shelf).
- **Consumes:** `thread.created` / `thread.renamed` / `thread.retired` from M2 (candidate-list maintenance); Digest one-liners from M2's digest job (classifier context); M5's morning-window signal (release of night Sparks and offline summary toast).
- **Explicitly out of scope for M1:** Pebbles, Briefings, Arcs, notification scheduling, calendar — Catch deposits; other modules spend.
