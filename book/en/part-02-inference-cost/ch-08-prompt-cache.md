# Ch 8 · Prompt cache（提示缓存）

Not semantic cache (“similar question → reuse answer”). Exact **prefix** KV reuse.

## Concept

**Definition.** Prompt cache is **byte-for-byte prefix** reuse of the transformer KV. A stable prefix (tools + system + early history) is hashed. Hit → cheaper/faster input. Change a byte before the breakpoint → miss from there on. Swap model → miss. It is **not** embedding similarity, not “cache the answer.”

**Why it exists.** Agent loops resend the same tool schema and system text every turn. Without prefix reuse, you pay full input on a growing window. Cache is the reason L0 “stability” in [Ch 12](../part-03-context-router/ch-12-l0-l4-window.md) is a *bill*, not taste.

**Mechanism — Anthropic (explicit).** Mark `cache_control: { type: "ephemeral", ttl?: "5m" | "1h" }` on content blocks. Max **4** breakpoints per request. Write: 5m **1.25×**, 1h **2.0×**. Read: **0.1×** base input (official). Workspace isolation on Claude API since 2026-02 (not org-wide sharing).

**Mechanism — OpenAI (mostly implicit).** Eligible models cache the longest exact prefix automatically; GPT-5.6+ can add an explicit breakpoint. Minimum ~1024 tokens. Write: GPT-5.6+ **1.25×** uncached; older often no write fee. Read: ~0.5× (gpt-4o) → 0.25× (gpt-4.1) → **0.1×** (gpt-5 family). TTL: GPT-5.6+ 30min exact; older best-effort 5–60min. `prompt_cache_key` shards similar traffic so they do not evict each other.

**Boundaries.** Compacting inserts a new block in the prefix → miss from that point ([Ch 14](../part-03-context-router/ch-14-compacting.md)). Rewriting system from intent every turn fights 0.1×. Semantic routers that embed the *query* do not hit this cache. LiteLLM must preserve breakpoint bytes or the hit dies at the gateway.

**Example — worked miss.** Insert a timestamp between stable instructions and the user turn → implicit prefix includes the timestamp → next request misses. Fix: explicit breakpoint *after* the stable block.

**Failure modes.** Tool schema churn in L0. Dumping full Skill bodies into system. `truncation: auto` dropping messages and breaking the prefix. Spraying one session across upstreams at a relay.

## Comparison — Anthropic explicit vs OpenAI implicit

| | Anthropic | OpenAI |
| --- | --- | --- |
| Mark | `cache_control: { type: "ephemeral", ttl?: "5m" \| "1h" }` | Automatic for eligible models; GPT-5.6+ optional explicit breakpoint |
| Breakpoints / req | max **4** | implicit at latest user/tool; explicit optional |
| Min prefix | model-dependent (often 1,024+) | ~1024 (older: 1024–2048) |
| Write | 5m **1.25×**; 1h **2.0×** | GPT-5.6+: **1.25×** uncached; older: often no write fee |
| Read | **0.1×** base input | ~0.5× (gpt-4o) → 0.25× (gpt-4.1) → **0.1×** (gpt-5 family) |
| TTL | 5m default, 1h optional | GPT-5.6+: 30min exact; older best-effort 5–60min |
| Affinity | workspace isolation (Claude API since 2026-02, not org-wide) | `prompt_cache_key` shards similar traffic |
| Match | literal prefix hash | exact prefix; **not** embedding similarity |

**Read (读):**
- [docs — Anthropic Prompt caching: official **0.1×** read, 1.25×/2.0× write, max 4 breakpoints; not semantic cache.](https://platform.claude.com/docs/en/build-with-claude/prompt-caching)
- [docs — OpenAI Prompt caching (API): exact prefix only; static first; `prompt_cache_key`; GPT-5.6+ write 1.25×.](https://developers.openai.com/api/docs/guides/prompt-caching)
- [docs — OpenAI Prompt Caching 201 cookbook: Flex vs Batch when cache hits matter; worked request patterns.](https://developers.openai.com/cookbook/examples/prompt_caching_201)
- [docs — Anthropic effective context engineering: why a stable prefix is an attention-budget tactic, not just a discount.](https://www.anthropic.com/engineering/effective-context-engineering-for-ai-agents)
- [docs — Claude cookbook on memory / compaction / tool clearing: three levers that all move the prefix you are trying to cache.](https://platform.claude.com/cookbook/tool-use-context-engineering-context-engineering-tools)
- [docs — OpenAI compaction guide: compacting rewrites history and therefore the cacheable prefix.](https://developers.openai.com/api/docs/guides/compaction)
- [paper — Lost in the Middle (Liu et al.): long windows fail in the *middle*; cache does not fix position bias.](https://arxiv.org/abs/2307.03172)

[Part II](./README.md)
