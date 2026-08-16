# Ch 7 · Control plane

Identity, billing, traces, gateway, enterprise wrap. The 9-step sketch only drew “Gateway”.

## Concept

**Definition.** The control plane is everything that is *not* the token stream but still decides whether a call happens: who is the principal (F1), what was logged (F2), which key/model/region (F3), which VPC/compliance envelope (F4). Gateway **retention** is often keys + invoice + traces, not the routing algorithm.

**Why it exists.** Cursor can skip F3 and call Anthropic/OpenAI. Multi-model shopping, China relay, and enterprise “one exit” make F3 required. Without F2, you cannot debug cache miss vs model swap vs tool-schema drift.

**Mechanism.** F1: org billing, SSO, commit contracts. F2: spans (LangSmith, Helicone, Langfuse) — prompt, tokens, cache hit, latency. F3: OpenRouter / LiteLLM / Portkey / one-api — translate dialects, pool keys, fallback. F4: VPC, 等保, DLP, Azure OpenAI contracts. China is not a translation of OpenRouter: 中转站, cloud plazas (百炼, 方舟), lab-direct APIs (DeepSeek, 智谱). 昇腾 ≠ CUDA.

**Boundaries.** Gateway ≠ Model Router ([Ch 18](../part-04-routing-nlu/ch-18-model-router.md) is *which model*; F3 is *how the call is issued*). Observability ≠ eval gate. MCP is a *tool protocol* (G3), not a gateway product. Relay economics: [Ch 10](../part-02-inference-cost/ch-10-relay-discount.md).

**Example.** Helicone in front of one OpenAI key is F2+F3 with routing as the door. A 中转站 that sprays each request to a different upstream kills prompt-cache spread — stickiness is the same discipline as L0.

**Failure modes.** Putting ACL in the system prompt and calling it F4. Using OpenRouter `auto` and thinking you have a task-aware context compiler. Logging secrets into F2 traces.

## Comparison

| Layer | Sellers | Bill |
| --- | --- | --- |
| F1 identity / billing | OpenAI org, Azure EA, WorkOS SSO | seats, commit, overage |
| F2 observability | LangSmith, Helicone, Langfuse | spans / month |
| F3 gateway | OpenRouter, LiteLLM, Portkey, one-api | markup or self-host software |
| F4 enterprise | VPC, 等保, DLP, Azure OpenAI contracts | cloud delta + audit |

| China role | Example | Layers |
| --- | --- | --- |
| Lab-direct API | DeepSeek, 智谱, Moonshot | C5+E2+E5 |
| Cloud plaza | 百炼, 方舟 | E2+F4 |
| Independent host | SiliconFlow | E2 |
| Relay | one-api, new-api | F3+F1 |
| Weight distribution | ModelScope, HF mirrors | C4+C5 |

**Read (读):**
- [docs — MCP architecture: tools as a protocol (G3), not a gateway SKU.](https://modelcontextprotocol.io/docs/learn/architecture)
- [repo — LiteLLM: self-host F3 proxy — retry, fallback, budget, dialect translate.](https://github.com/BerriAI/litellm)
- [docs — OpenRouter: hosted multi-model F3 with unified billing.](https://openrouter.ai/docs)
- [repo — Portkey gateway: open-source AI gateway (keys, fallback, guardrails).](https://github.com/Portkey-AI/gateway)
- [repo — Helicone: observability-first proxy; F2 with F3 as the door.](https://github.com/Helicone/helicone)
- [repo — Langfuse: open-source LLM tracing (F2).](https://github.com/langfuse/langfuse)
- [docs — WorkOS SSO: F1 identity plumbing for apps, not a model router.](https://workos.com/sso)
- [docs — 阿里云百炼: China cloud plaza (E2+F4), not OpenRouter-in-Chinese.](https://www.aliyun.com/product/bailian)

[Part I](./README.md)
