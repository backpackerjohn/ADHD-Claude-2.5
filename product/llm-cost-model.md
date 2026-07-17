# LLM Cost Model — Guardrail 6 Compliance

**Requirement:** at *high* usage, LLM API cost per user must stay under **$5.99/month**, so a ~$8.99/mo price is profitable.

## Pricing basis (Anthropic API, reference cached 2026-06-24)

Source: Anthropic model pricing per the Claude API reference (live page: https://platform.claude.com/docs/en/pricing.md). Rates per million tokens (MTok):

| Model | Input | Output | Role in our app |
|---|---|---|---|
| Claude Haiku 4.5 (`claude-haiku-4-5`) | $1.00 | $5.00 | **Workhorse**: capture triage/classification, auto-filing, reminder phrasing, short summaries |
| Claude Sonnet (`claude-sonnet-4-6` / `claude-sonnet-5`) | $3.00 | $15.00 | **Quality tier**: task breakdown, re-entry briefings, weekly review synthesis (Sonnet 5 intro pricing $2/$10 through 2026-08-31) |
| Claude Opus 4.8 | $5.00 | $25.00 | Not used in-product (cost); reserved for offline prompt development |

Cost levers from the same reference:
- **Prompt caching**: cache reads ≈ 0.1× input price; cache writes 1.25× (5-min TTL). Our per-user system prompt + user-context block is stable → cache it.
- **Batch API**: 50% off all tokens for async jobs — used for overnight/next-morning jobs (morning brief pre-generation, weekly digests).
- Equivalent-tier models from other providers (GPT-4o-mini, Gemini Flash) sit at or below Haiku-class pricing, so this model is provider-conservative. [INFERENCE — competitor prices not independently verified in this environment; the model is built entirely on Anthropic list prices.]

## High-usage user profile (deliberately heavy)

Assumes a power user engaging many times daily, 30 days/month. Token sizes include prompt overhead after caching (cached system/context ≈ 2,000 tokens, billed at 0.1× on reads).

| Feature (module) | Model | Calls/mo | In tok/call (uncached + cached@0.1×) | Out tok/call | Cost/call | Monthly |
|---|---|---|---|---:|---:|---:|
| Capture triage & auto-filing (30/day) | Haiku | 900 | 300 + 2,000@0.1× = 500 eff. | 120 | $0.0011 | **$0.99** |
| Task/project breakdown, standard (4/day) | Haiku | 120 | 800 + 2,000@0.1× = 1,000 eff. | 400 | $0.0030 | **$0.36** |
| Task breakdown, complex projects (1/day) | Sonnet | 30 | 1,500 + 3,000@0.1× = 1,800 eff. | 700 | $0.0159 | **$0.48** |
| Re-entry briefing (2/day) | Sonnet | 60 | 2,500 + 3,000@0.1× = 2,800 eff. | 350 | $0.0137 | **$0.82** |
| Freeze-moment "next tiny step" nudges (3/day) | Haiku | 90 | 600 + 2,000@0.1× = 800 eff. | 150 | $0.0016 | **$0.14** |
| Morning brief (1/day, Batch API −50%) | Sonnet | 30 | 3,000 eff. | 500 | $0.0083 | **$0.25** |
| Evening auto-close / rollover (1/day, Batch) | Haiku | 30 | 1,500 eff. | 300 | $0.0015 | **$0.05** |
| Weekly review synthesis (Batch) | Sonnet | 4 | 8,000 eff. | 1,500 | $0.0233 | **$0.09** |
| Search/RAG answer over own notes (3/day) | Haiku | 90 | 2,000 eff. | 250 | $0.0033 | **$0.29** |
| Voice-note transcription cleanup (5/day) | Haiku | 150 | 700 eff. | 300 | $0.0022 | **$0.33** |
| **Subtotal** | | | | | | **$3.80** |
| + 25% safety margin (retries, longer contexts, growth) | | | | | | **$0.95** |
| **Total high-usage LLM cost** | | | | | | **≈ $4.75/mo** |

**Verdict: ✅ under the $5.99 ceiling with ~21% headroom**, at a genuinely heavy usage profile (~1,500 LLM calls/month).

## Sensitivity & guards

- **Median user** engages far less (mental-health app median 30-day retention ≈ 3.3% — [Baumel et al. 2019](https://www.jmir.org/2019/9/e14567/); even retained users won't sustain 50 interactions/day). Expected blended cost across paying users: **$1–2/mo**. [INFERENCE from usage-distribution norms]
- **Abuse guard**: soft cap of ~2,000 AI actions/month (≈ 65/day) before graceful degradation to Haiku-only + queued processing; keeps worst-case ≤ $5.99.
- **Voice transcription** (speech-to-text) is a separate non-LLM cost (~$0.006/min via typical STT APIs [INFERENCE — verify at build time]); at 10 min/day ≈ $1.80/mo, still within an $8.99 price's gross margin; on-device transcription (iOS/Android built-in) reduces this toward $0.
- **Price/margin**: at $8.99/mo, LLM ≈ $4.75 worst-case leaves $4.24 for infra + margin per high-usage user; blended margin much higher. At a $6.99 price point the model still clears worst-case LLM cost.
- **Model drift**: prices above are list prices as of the cached reference; per-token prices have historically fallen, and any equivalent-tier substitution (e.g., Gemini Flash-class) only lowers cost.

## Architectural rules that keep this true

1. **Haiku-first routing**: every call defaults to Haiku; escalation to Sonnet only on complexity triggers (project ≥ N steps, re-entry gap ≥ 7 days, user taps "go deeper").
2. **Stable cached prefix**: frozen system prompt + slowly-changing user profile block behind a cache breakpoint; volatile content after it.
3. **Batch everything non-interactive**: morning briefs, rollovers, weekly reviews run as overnight batch jobs at 50% price.
4. **No LLM where none is needed** (see ai-spec.md): timers, reminders, streak-free progress math, search indexing, and notification scheduling are deterministic code, not model calls.
