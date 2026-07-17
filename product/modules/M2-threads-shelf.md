# Module: M2 — Threads & Shelf

> Conforms to `product/modules/_TEMPLATE.md`. Terminology, objects, and states per
> `product/product-brief.md` (source of truth). Basis: `contest/winner.md` synthesis, `contest/thesis-B.md` §5 M2.

## 1. Purpose

M2 is Ember's living memory and its browse/find surface — founder mandate #3 (view, pick up, find). Every Spark, Pebble, decision, and Breadcrumb accrues to a **Thread**; the **Shelf** is where Maya sees everything she has going, everything resting, and everything honorably finished. It exists because the verified wedge is re-entry friction plus upstream context loss: "you open the document and can't remember where you were or what you were thinking" ([Fabric](https://fabric.so/blog/how-to-finish-projects-when-your-brain-keeps-starting-new-ones)), "restarting harder than starting something new" ([ADHD Philadelphia](https://www.adhdphiladelphia.com/blog/why-adults-with-adhd-lose-momentum-so-easily-after-interruptions)), and "I know I wrote it down somewhere… but is it in my Apple Notes, my Kindle highlights, Scrivener…?" ([Passionate Writer Coaching](https://passionatewritercoaching.com/best-free-note-taking-apps/)). The Shelf's amnesty framing answers the evidenced harm of streak/overdue shame mechanics ([Kabit](https://kabitapp.com/blog/habit-tracker-adhd), [medRxiv streak-backfire preprint](https://www.medrxiv.org/content/10.1101/2024.12.26.24319676.full.pdf)) and the graveyard pattern of projects stalling at 80% ([Tiimo](https://www.tiimoapp.com/resource-hub/finishing-what-you-start-adhd)). Because absence is the norm (30-day category retention ≈3.3%, [Baumel et al. 2019](https://www.jmir.org/2019/9/e14567/)), M2 is designed so a shelf untouched for weeks reads as safe-keeping, never as debt.

## 2. User goals

- I want everything I've ever captured about a project gathered in one chronological story, so that returning doesn't feel like starting from zero.
- I want to see what's warm right now and what's resting without guilt, so that paused never reads as failed.
- I want to find a half-remembered thought by meaning, words, or era ("around the time I was into climbing"), so that "I know I wrote it down somewhere" always resolves.
- I want to merge duplicate threads and re-home misfiled Sparks in seconds, so that AI mistakes never cost me trust in zero-decision capture.
- I want my finished and retired projects displayed as history with Closing Notes, so that my graveyard becomes a gallery, not a shame wall.
- I want to see what Ember knows about a thread, so that the memory working for me is inspectable, never spooky.

## 3. Objects

| Object | M2 role | Fields owned by M2 |
|---|---|---|
| **Thread** | **Owned.** The living container | id, title, type (`project` · `idea cluster` · `life-admin arc` · `person`), state (`warm` · `resting` · `finished` · `retired`), emoji/cover, created_at, last_touched_at, warmth score, pinned, merged_from[] |
| **Digest** | **Owned.** AI-maintained per-Thread summary | digest text (structured), last_rebuilt_at, staleness flag, user_viewed flag |
| **Closing Note** | **Owned.** Honorable retirement/finish note | note text, written_at, thread_id, tone-checked flag |
| **Spark** | Touched (owned by M1) | thread_id assignment, position in story, resurfaced/archived flags |
| **Pebble** | Touched (owned by M4/M3) | rendered inline in story with done/dissolved outcome |
| **Breadcrumb** | Touched (auto-written cross-cutting) | rendered inline; immutable |
| **Briefing** | Touched (owned by M3) | past Briefings rendered inline as story milestones |

**Thread story anatomy (THREAD-01).** A Thread opens as a chronological "story of this project," newest at the bottom, composed of typed entries rendered inline in one stream:

- **Sparks** — the user's own words (text, or voice with transcript + play chip), each stamped with time and capture surface;
- **Pebbles** — suggested → accepted → done/dissolved, shown as quiet milestones ("did the 5-minute 1099 search"), never as unchecked debt;
- **Decisions** — Sparks or Briefing outcomes the Digest has recognized as decisions get a subtle "decided" marker ("going with the blue-green palette"), so the story's turning points are scannable;
- **Breadcrumbs** — auto-written stop-notes in a distinct hand ("You stopped after drafting the email to the accountant…"), immutable;
- **Briefings** — past Warm Starts kept inline as "you came back here" moments, collapsible;
- **Closing Notes** — if the Thread was ever finished/retired and restored, the note stays in the timeline.

Header: title, type badge, state, warmth indicator, "What Ember knows" menu item, and the Arc chip when M4 has an active Arc. Day/month separators; a "jump to when…" scrubber for long stories. Thread **types** share one anatomy but tune defaults: `project` (Arc-capable, deadline chips), `idea cluster` (loose, merge-prone, dedupe suggestions more active), `life-admin arc` (deadline-forward, e.g. taxes/move/job hunt), `person` (gift ideas, conversations to have; no Pebble pressure by default).

## 4. Lifecycle

**Thread:** created by (a) M1 auto-filing proposing a new Thread, (b) explicit "New thread" on the Shelf, (c) graveyard import clustering, (d) merge (survivor absorbs). Transitions:
- `warm → resting`: manual ("Let it rest") or **auto-rest after N quiet days** (default N=14, user-adjustable 7–45). Copy is amnesty-only: "Moved to the resting shelf — it'll keep warm here." Never "inactive," "stale," or "abandoned."
- `resting → warm`: any touch — new Spark filed, story opened, Pebble accepted, or Warm Start acted on — rekindles it. No ceremony, no "you're back!" guilt framing.
- `warm/resting → finished`: user marks done; AI drafts a Closing Note (celebratory variant) for the gallery.
- `warm/resting → retired`: "Retire with honor" (from Shelf or a Briefing's fourth button); AI writes the Closing Note ("You built the hard part. It taught you resin casting.").
- `finished/retired → warm`: **Restore rekindles.** The Closing Note is kept as a story entry ("Retired in March — restored today"), the Digest is refreshed, and the Thread returns to the Warm row with a Warm Start queued.
- Deletion: explicit only, double-confirmed, 30-day undo window; merge never deletes (sources are tombstoned with pointers). Retention: story history kept indefinitely; raw story pages beyond 12 months move to cold archival storage, transparently rehydrated on scroll.

**Digest:** created with the Thread; rebuilt by nightly Batch job when the Thread changed that day; marked stale (and rebuilt on demand) after merges, bulk moves, or restores. Never deleted while its Thread exists.

**Closing Note:** created at finish/retire; editable by the user; kept in the gallery; preserved in-story after restore.

## 5. Actions

| Action | Trigger surface | Input | Effect | Undo |
|---|---|---|---|---|
| Open thread story | Tap Thread card; `Enter` on web | — | Chronological story view; if time-away ≥ threshold, M3 Briefing renders on top | n/a |
| New thread | Shelf "+"; web `N` | Title (optional — AI suggests from first Spark), type | Creates warm Thread | Delete within undo window |
| Rename / retype / set cover | Thread header menu | Text / type picker / emoji | Metadata edit | Undo toast |
| Let it rest | Thread menu; swipe on Shelf card | — | `→ resting`, amnesty copy | One-tap rekindle |
| Pick it back up | Tap resting Thread | — | Opens story; any touch `→ warm` | Rest again |
| Finish | Thread menu | Optional final note | `→ finished`, Closing Note drafted | Restore |
| Retire with honor | Thread menu; Briefing button 4 | — | `→ retired`, Closing Note written | Restore (rekindles) |
| Restore | Retired/Finished gallery card | — | `→ warm`, Digest refresh, Warm Start queued | Re-retire |
| Search | Shelf search bar; web `/` | Natural language / keywords | Hybrid results (see §8) | n/a |
| Filter | Filter chips | Type, state, has-deadline (Arc), last-touched era | Filters Shelf/search | Clear chips |
| Move Spark | Drag Spark to another Thread; long-press → "Move" | Target Thread | Refiles; logs correction as M1 training signal | Undo toast |
| Bulk-select | Long-press + tap; web `Shift`-click | Sparks or Threads | Enables bulk bar | Deselect |
| Bulk move Sparks | Bulk bar → "Move to…" | Target Thread | Refiles set; one training signal batch | Single undo |
| Merge threads | Bulk bar → "Merge" (2+ Threads) | Pick survivor + title | Stories interleaved chronologically; Digest rebuilt; sources tombstoned | Un-merge within 30 days |
| Bulk-rest | Bulk bar → "Let these rest" | Thread set | All `→ resting` | Single undo |
| Archive Spark | Spark menu | — | Hidden from story, still searchable ("include archived" toggle) | Restore |
| View Digest | Thread menu → "What Ember knows about this thread" | — | Read-only Digest panel + "Correct something" affordance | n/a |
| Adjust auto-rest | Shelf settings | N days (7–45) or off | Changes quiet-day threshold | Revert |

## 6. States

- **Empty / first-run (SHELF-00):** No debt framing, no setup wizard (setup is where Notion-style systems die — "an organizational decision at the moment of every save" [MindStash](https://www.mindstash.app/blogs/why-your-adhd-brain-hates-notion-and-what-actually-works-instead)). One warm card: "Your shelf is empty — catch one thought and Ember starts a thread for you." Import-the-wreckage entry point beneath. No sample clutter.
- **One thread (SHELF-01):** Single card in Warm row; Resting/Finished sections hidden until first occupied (no empty racks implying obligation).
- **Ideal (SHELF-02):** Warm row (max ~7 visible, warmth-ordered), Resting shelf below ("Resting — paused isn't failed"), Finished & Retired gallery behind one tap, each card showing its Closing Note's first line.
- **50+ threads (SHELF-03):** Warm row capped; Resting shelf groups by era ("This spring," "Last year"); type filter chips surface; search promoted to top.
- **Loading:** Skeleton cards; cached shelf renders instantly from local store, syncs behind.
- **Search-no-results (SHELF-04):** "Nothing surfaced — but nothing is lost." Offers: broaden to archived Sparks, widen the time window, or fuzzier semantic pass. Never a blank void.
- **Merge-conflict (MODAL-M2-01):** Both sources have Arcs or conflicting titles/deadlines → modal asks which Arc survives (other pauses, kept in story); no silent data loss.
- **Error:** Story page fails to load → "This part of the story is taking a moment" + retry; local Sparks always visible.
- **Offline:** Shelf and cached stories readable; keyword search works locally; semantic search greyed with "back online soon" note; state changes queue.
- **Returning after weeks (SHELF-05):** Shelf opens beneath a welcome-back banner ("Welcome back. Nothing is lost."); nothing red, nothing counted; threads auto-rested during absence sit calmly on the Resting shelf; the Doorway (M5) handles the welcome-back card, the Shelf just looks safe.

## 7. Workflows

Screens referenced: SHELF-00…05, THREAD-01, MODAL-M2-01 (merge conflict), PANEL-M2-01 (Digest viewer), GALLERY-01 (Finished & Retired).

**W1 — Find and pick up (happy path):** Shelf (SHELF-02) → search "logo idea, around when I was into climbing" (§8 temporal anchor resolves to that era) → result: Spark in *Etsy shop* Thread → open story (THREAD-01) → M3 Briefing on top (time-away ≥3 days) → tiny step accepted → Thread `→ warm`.
*Failure A — no results:* SHELF-04 → "include archived" → found in archived Sparks → restore to story.
*Failure B — found in wrong Thread:* long-press → Move to *Etsy shop* → undo toast → correction logged to M1 classifier.

**W2 — Merge duplicates:** Maya notices *Etsy shop* and *shop logo* are one project → bulk-select both → Merge → survivor picker (MODAL-M2-01 if both have Arcs) → interleaved story, tombstones, Digest rebuild queued → toast "Merged — un-merge anytime this month."
*Failure — wrong merge:* Un-merge restores both Threads from tombstones with their original stories; Sparks added post-merge stay with the survivor and are flagged for one-drag re-homing.

**W3 — Return after three weeks away:** push notification? None from M2 (M5 owns the welcome-back Doorway). Maya opens the app → SHELF-05: banner "Welcome back. Nothing is lost.", Warm row holds whatever stayed warm, threads that auto-rested during the absence sit on the Resting shelf with era labels → she taps a resting Thread → story opens beneath a full (≥14-day) Briefing → one touch rekindles it to warm. Nothing anywhere counts the days she was gone.
*Failure — everything auto-rested (empty Warm row):* Warm row shows "Everything's resting — pick anything up whenever" over the Resting shelf; never an empty-state that reads as failure.

**W4 — Retire and rekindle:** From a Briefing, "Retire with honor" → Closing Note appears for edit → Thread to gallery. Months later, gallery card → Restore → rekindled to Warm row, Digest refreshed, Warm Start queued ("You retired this in March. Here's what you had…").

## 8. AI behavior

| Job | Trigger | Model tier | Contract |
|---|---|---|---|
| **Digest maintenance** | Nightly Batch, changed Threads only; on-demand after merge/restore | Haiku, Batch (50% off) | In: prior Digest + day's deltas (never full raw history). Out: structured Digest — where things stand, open questions, key decisions w/ dates, verbatim quote candidates, why-she-cared. Guardrail: quotes verbatim-only; no invented facts; warm second person; banned words per brief enforced by lint. |
| **Semantic search** | Query issued | Embeddings (+ Haiku rerank on ambiguity) | Hybrid: keyword (exact/fuzzy) + vector similarity + **temporal anchoring** — "around the time I was into climbing" resolves the era via the climbing Thread's activity window, then time-weights all results. Results always show why matched (term hit, meaning, or era). |
| **Merge assistance** | Merge confirmed | Haiku | Proposes survivor title, unified type, rebuilt Digest; flags contradictions to MODAL-M2-01 rather than resolving silently. |
| **Closing Notes** | Finish/retire | Sonnet | Warm, specific, honors what was built/learned; user-editable; never generic praise. |
| **Duplicate suggestion** | Digest job detects high inter-Thread similarity | Haiku | At most one gentle "these might be one thread" chip on the Shelf; dismissible, never nags again for that pair. |

**Digest visibility:** hidden by default (Briefings are the human-facing product of it), but always viewable via "What Ember knows about this thread," with a correction affordance — inspectability is the R3 trust mitigation.
**Fallbacks:** AI down → keyword+recency search still works; Digest marked stale and Briefings (M3) say so; Closing Note falls back to a template quoting the last three Sparks. Low-confidence merge/dedupe suggestions are simply not shown.
**Deliberately NOT AI:** state transitions, auto-rest day-math, warmth ordering (deterministic: recency + Pebble activity + Arc deadlines), pagination, undo/restore, tombstones, archival.

## 9. Scale

Targets: **100+ Threads, 10k Sparks, 3 years of history** without degradation.
- **Shelf:** renders from a local index (id, title, state, warmth, cover) — O(shelf) not O(history); full shelf paint <300ms at 200 Threads.
- **Story lazy-loading:** newest page first (~50 entries), infinite scroll upward; year markers as jump points; months >12 old served from cold archive, rehydrated per page (<1s budget) with an inline "reaching back to 2024…" shimmer.
- **Search:** embeddings computed at capture (M1), stored locally + server; hybrid query <1.5s at 10k Sparks; results paginated 20/page.
- **Digest economics:** nightly job touches changed Threads only (typically <10/night); Digest size capped (~2k tokens) so Briefing context stays bounded regardless of history length — the digest-first architecture from R6.
- **Gallery:** virtualized grid; Closing Notes load lazily.
- **Cost:** per-user AI spend within the $5/mo killswitch envelope (brief AI summary); search embeddings ≈$0.05/mo at high usage (thesis-B §8).

## 10. Errors

- **Sync conflict (two devices):** story entries are append-only → union merge, no loss; conflicting Thread-state changes resolve last-writer-wins with a quiet story entry ("rested on phone, rekindled on web — staying warm").
- **AI misfiling** (R3): every filed Spark shows its Thread chip; one-drag move; corrections train M1. Low-confidence items never enter a Thread silently — they sit in M1's Loose Sparks tray.
- **Bad merge:** 30-day un-merge from tombstones (see W2).
- **Digest wrong:** user "Correct something" note is pinned into the next rebuild's input; Briefings prefer user corrections over inferred content.
- **Partial write** (capture synced, embedding failed): Spark visible immediately; embedding retried in background; meanwhile findable by keyword.
- **Accidental retire/rest/archive/delete:** every destructive-feeling action has a one-tap undo toast; delete has a 30-day window; restore-from-retired is a first-class action, not a recovery hack.
- **Cold-archive fetch failure:** recent story remains usable; older pages show retry affordance — never a blank story.

## 11. Permissions

Private by default, single-user; no sharing or collaboration in v1 (person-type Threads are *about* people — notes on Mom's birthday ideas — and are never visible to them; onboarding says so explicitly). ADHD is health-adjacent data: Thread stories, Digests, and search queries are encrypted in transit and at rest; embeddings and Digests are derived data deletable with the account; no training on user content without opt-in; export-everything (JSON + Markdown) available from settings. Deleting a Thread purges its story, Digest, Closing Note, and embeddings after the 30-day undo window. Auto-pause of billing (cross-cutting) never touches or degrades stored memory: "we paused your billing — everything is safe."

## 12. Dependencies

- **Consumes:** M1 Catch (filed Sparks + confidence, thread-creation proposals, import clusters); M3 (Briefings rendered atop stories; Retire-with-honor button); M4 (Arc state for deadline chips and merge-conflict handling); M5 (Doorway deep-links into stories); cross-cutting auto-Breadcrumb writer (story entries); billing status (read-only).
- **Emits:** `thread.state_changed` (M3 Briefing scaling, M5 Doorway pool, billing auto-pause idle signal), `spark.moved` / `bulk.moved` (M1 training signal), `thread.merged` / `thread.restored` (Digest rebuild queue, M4 Arc reconciliation), `digest.updated` (M3/M5 context freshness), `closing_note.created` (gallery, M5 "look what you finished" candidates).
- **Services:** embedding + vector store, nightly Batch pipeline (Haiku digests), cold archival storage, local-first cache/sync.
