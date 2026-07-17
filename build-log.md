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
