# Ch 18 · Task → Model Router

Independent problem: **which model** for this task. Binary (strong vs weak) or k-way (code-edit / long-reason / chat / vision / cheap-classify).

## Concept

**Definition.** Router = the decision of *which model*. Gateway = keys, protocol translate, traces, breakers ([Ch 7](../part-01-value-chain/ch-07-control-plane.md), [Ch 19](./ch-19-api-switching.md)). Swap model **breaks** prompt cache and OpenAI encrypted compact blobs. Read CONTINUE/SHIFT from the context layer; do not reverse-fuse the two nets.

**Why it exists.** Strong models are expensive. Most turns do not need them. RouteLLM / FrugalGPT exist because *quality-at-a-cost* is a measurable trade, not a vibe.

**Mechanism.** Rules (token count, code fences) cost 0 ms. Learned (RouteLLM MF / BERT / causal clf on Arena prefs) 10–40ms. Cascade (FrugalGPT): weak first, escalate if a quality score fails — may pay a second full generation. ETH 2024: when routing vs cascading is optimal; bottleneck is the quality estimator. Online bandits (BaRP / PILOT) move the policy. Semantic (vLLM Semantic Router): query embed → table.

**Boundaries.** Not context packing. Not intent NLU (though product intents can be *features*). LiteLLM fallback chains are *health* routing, not task routing. OpenRouter `auto` is a hosted router with *their* objective, not yours.

**Example.** RouteLLM (LMSYS, ICLR 2025): in *their* setting, matrix-factorization routing ≈ **95% GPT-4 quality** while sending ~**14%** of requests to the strong model. Arena/MT-Bench numbers — not your traffic.

**Failure modes.** Swapping models on CONTINUE (cache dies). Using Arena routers on SWE-bench traffic. Cascading every request so you pay weak+strong.

## Comparison

| Family | Example | When | Latency tax |
| --- | --- | --- | --- |
| Rules | token count, code fences, plan, file count | before request | 0 |
| Learned | RouteLLM: MF / BERT / causal clf on Arena prefs | before | 10–40ms |
| Cascade | FrugalGPT: weak first, escalate if score fails | maybe a second full gen | one weak generation |
| Cascade-routing | ETH 2024: route then maybe escalate | hybrid | depends on quality estimator |
| Online bandit | BaRP / PILOT | before; policy moves | 0 extra forward |
| Semantic | vLLM Semantic Router: query embed → table | before | one embed |

Implementations: LiteLLM (self-host retry/fallback/budget), OpenRouter `openrouter/auto`, Portkey, Not Diamond.

**Read (读):**
- [paper — RouteLLM (Ong et al., ICLR 2025): preference-data routers; ~95% GPT-4 quality at ~14% strong-model share *on their benches*.](https://arxiv.org/abs/2406.18665)
- [repo — lm-sys/RouteLLM: official implementation of those routers.](https://github.com/lm-sys/RouteLLM)
- [docs — LMSYS blog 2024-07-01: RouteLLM announcement and the 95%/14% headline in context.](https://lmsys.org/blog/2024-07-01-routellm/)
- [paper — FrugalGPT (Chen et al. 2023): cascade — cheap model first, escalate.](https://arxiv.org/abs/2305.05176)
- [paper — A Unified Approach to Routing and Cascading (Dekoninck et al., ETH 2024): when each is optimal; quality estimators bottleneck.](https://arxiv.org/abs/2410.10347)
- [repo — vllm-project/semantic-router: query-signal Mixture-of-Models router, not a marketplace gateway.](https://github.com/vllm-project/semantic-router)
- [repo — BerriAI/litellm: gateway fallback/budget; health routing, not task routing.](https://github.com/BerriAI/litellm)
- [docs — OpenRouter: hosted `auto` router plus unified billing.](https://openrouter.ai/docs)
- [repo — Portkey-AI/gateway: gateway with routing hooks.](https://github.com/Portkey-AI/gateway)
- [repo — Helicone/helicone: observability door in front of whatever router you picked.](https://github.com/Helicone/helicone)

[Part IV](./README.md)
