# Ch 10 · Relay & enterprise discount

Relay（中转站） margin is **upstream spread**, not a self-trained model. Shape: “your app eats your enterprise price”, not “split an Anthropic enterprise key into retail keys”.

## Concept

**Definition.** A relay resells *access* to someone else’s inference. Legal channels: contract tiers, official cache/Batch/Flex multipliers, startup credits. The spread is the difference between what the relay pays upstream and what it charges you. It is **not** a new model.

**Why it exists.** Apps want one key, one invoice, maybe China reach. Labs sell volume commits. The gap is a market — and a compliance trap.

**Mechanism.** Enterprise API: annual commit / volume, often **5–40% off** list (your contract, not a blog). Prompt cache: repeat prefix at 0.1×–0.5× input → inference savings that *can* become spread if the prefix is sticky. Batch official **50%**. Flex ~50% realtime. Cloud / ISV / edu credits: expiry, product-bound, no-resale. If the relay sprays each request to a different upstream or rewrites the prefix, cache spread vanishes. Stickiness = same discipline as L0.

**Boundaries.** This chapter does **not** list cracked-key markets. Enterprise contracts usually **forbid reselling the key** to unsigned end users. A 中转站 is F3+F1, not C1. OpenRouter is a documented multi-model gateway with its own terms — not the same legal object as an unofficial relay.

**Example.** A company on an Anthropic commit, calling through its own LiteLLM with a frozen L0, can stack enterprise % and 0.1× cache *for itself*. Splitting that key to anonymous retail users is a contract violation, not a SKU.

**Failure modes.** Advertising “0.1×” while rotating upstreams every request. Treating startup credits as resale inventory. Ignoring KYC clauses because the HTTP looks like OpenAI-compat.

## Comparison — where the spread comes from

| Source | Mechanism | Who pays | Constraint |
| --- | --- | --- | --- |
| Enterprise API | annual commit / volume, often **5–40% off** list | lab sales | min spend, invoice entity, residency |
| Prompt cache | repeat prefix at 0.1×–0.5× input | inference savings → spread | byte-stable prefix; model swap = miss |
| Cloud credits | startup / marketplace coupons | cloud / lab marketing | expiry, product-bound, no-resale |
| ISV / edu subsidy | official token grants | growth budget | KYC, audit |
| Batch | official **50%** | fill the trough | ~24h; not the agent loop |
| Flex | ~50%, realtime API | off-peak | higher latency |

China channel table: [Ch 7](../part-01-value-chain/ch-07-control-plane.md).

**Read (读):**
- [docs — Anthropic prompt caching: the official **0.1×** read a relay can only keep if the prefix is sticky.](https://platform.claude.com/docs/en/build-with-claude/prompt-caching)
- [docs — OpenAI Batch: official **50%** — a list-price SKU, not a side deal.](https://platform.openai.com/docs/guides/batch)
- [docs — OpenAI Flex: official ~50% realtime discount.](https://platform.openai.com/docs/guides/flex-processing)
- [docs — Anthropic commercial terms (current public terms): resale and acceptable-use live in the contract, not a blog.](https://www.anthropic.com/legal/commercial-terms)
- [docs — OpenAI usage policies: what an API customer may not do with the key.](https://openai.com/policies/usage-policies)
- [repo — LiteLLM: the self-host F3 many enterprises use instead of an unofficial relay.](https://github.com/BerriAI/litellm)
- [docs — OpenRouter docs: a documented multi-model gateway with its own billing, not a cloned enterprise key.](https://openrouter.ai/docs)

[Part II](./README.md)
