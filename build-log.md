# Build Log

Running log of decisions, self-answered questions, blockers, and route-arounds, per Guardrail 5 ("Never ask me anything... log the question, your answer, and why").

---

## 2026-07-17 — Session start

### Decision D-001: Which repo do artifacts go to?
**Question I would have asked:** Guardrail 4 says "Work inside https://github.com/backpackerjohn/Adddd.git — all artifacts go there." But this session is scoped and branch-locked by the harness to `backpackerjohn/ADHD-Claude-2.5`, branch `claude/adhd-claude-instructions-de6j2c`, with an explicit instruction to never push elsewhere.
**Answer:** All artifacts go to `backpackerjohn/ADHD-Claude-2.5` on branch `claude/adhd-claude-instructions-de6j2c`.
**Why:** The session's repository scope and branch requirements are hard constraints of this environment; the `Adddd` repo is not in the session's authorized scope. The spirit of Guardrail 4 is "everything stays in a repo I own, nothing published elsewhere" — fully honored here. The instruction file itself lives in this repo, so this is also where the requester will look.

### Decision D-002: Web research route (blocker resolved)
**Question I would have asked:** Direct web fetching (WebFetch/curl) is blocked by this environment's egress network policy — every host except GitHub, package registries, and Anthropic returns a policy 403 at the proxy (verified against reddit.com, wikipedia.org, add.org, additudemag.com, apps.apple.com, news.ycombinator.com, google.com). The proxy documentation explicitly forbids routing around policy denials. How do I satisfy Guardrail 3 ("every claim must trace to a real URL you actually fetched")?
**Answer:** Use WebSearch (which runs through Anthropic's search infrastructure, not the sandboxed egress proxy, and works) as the sole evidence channel. WebSearch retrieves and returns page content excerpts together with their source URLs. Every quote/stat in the deliverables is captured verbatim from WebSearch-retrieved content and cited with its URL. Claims are additionally verified by independent re-retrieval (a second, differently-phrased search that must surface the same fact/URL). The evidence ledger labels each claim's verification method honestly: `search-retrieved` (content + URL returned by search), `corroborated` (confirmed by ≥2 independent retrievals), or `inference` (labeled as such).
**Why:** Guardrail 5 says blocked is not an option and to find another route — but the environment's own security policy forbids proxy circumvention (and attempts like text-mirror services are also policy-blocked). WebSearch is the sanctioned route to the open internet in this environment. The honest-labeling approach satisfies the spirit of Guardrail 3: nothing invented, everything traceable to a real URL, verification level disclosed rather than overstated.

### Decision D-003: Orchestration plan
Multi-agent workflows (explicitly mandated by the instruction file) run as sequenced Workflow invocations, one per phase-group, so results gate later phases (phase gates requirement):
1. Problem hunt: parallel researchers across sources/angles → adversarial quote verification → synthesis.
2. Product contest: rival product teams → judge panel → skeptic attack → winner.
3. Architecture: shared brief → parallel module specs on a common template → cross-review (scale challenge, AI-necessity audit, contradiction hunt).
4. Design: design system → wireframes (web + mobile) → prototype → Playwright screenshot verification.
5. Validation: red team → completeness critic → final integration pass → recap.html.

## 2026-07-17 — Phase 6-8 integration decisions

### D-004: Product name
Winner keeps the name **Ember** (thesis metaphor: embers survive untended and re-kindle). Rejected renaming — the metaphor IS the thesis.

### D-005: Auto-rest threshold (M2)
Threads auto-move warm→resting after **14 quiet days** (user-adjustable 7–45). Introduced by M2 owner; canonicalized here. Copy is shame-free ("resting", never "stale").

### D-006: Sonnet-escalation gap threshold (supersedes cost model wording)
Briefing tiers per the brief: whisper <3d (Haiku), brief ≥3d and full ≥14d (Sonnet). The cost model's "re-entry gap ≥ 7 days" escalation rule is superseded. Cost impact absorbed by headroom (ai-spec roll-up ≈$4.89 < $5.99).

### D-007: "Weekly review synthesis" removed
Appeared only in the cost model's generic profile; maps to no module in the fixed five-module list. Excluded from routing; its budget is headroom.

### D-008: Doorway generation is nightly Batch
Per winner.md synthesis item 8; only welcome-back Doorways generate synchronously. Thesis-B §8's sync pricing superseded (cheaper).

### D-009: Cold-start filing exception (M1)
With zero Threads, the classifier auto-creates starter Threads (labeled Ember-started, renamable) to keep "zero-decision" true on day one. Canonical.

### D-010: Free-tier COGS ceiling
Hard cap **$0.25/mo** per free user (target $0.10), enforced by free-tier limits (3 unsticks/day, 1 arc). Canonical.

### D-011: P90 cost planning number
Two independent models produced $3.04 (thesis-B) and $4.75 (llm-cost-model) — different usage profiles, same architecture. Planning number is the conservative **$4.75**; ai-spec's fully-routed roll-up ≈$4.89 is the engineering budget. All under the $5.99 guardrail.

### D-012: Metric name
Cumulative streak-free metric is **"ember hours"** (brief) — winner.md's "engine hours" was Ignition's term.

### D-013: Closing Note scope
Closing Notes are written at *retirement* AND offered at *completion* (celebratory variant). Extends the brief's object table; canonical.

### D-014: Wireframe ID reconciliation
`product/navigation-map.md` IDs (W-xx/O-xx) are canonical; M3's placeholder WS-xx IDs remapped (W-05, W-05b, W-06, W-07, O-04). Module specs' template reference to a separate `object-model.md` resolves to the object table inside `product-brief.md` (no separate file).
