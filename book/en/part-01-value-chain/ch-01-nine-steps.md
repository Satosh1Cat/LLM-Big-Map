# Ch 1 · The 9-step sketch

Oral map: data → train → LLM → deploy → inference → API → gateway → app → user.

## Concept

**Definition.** A 30-second alignment map of the LLM *industry*, not a programming tutorial. Each box *can* be a firm, a billable unit, and a failure mode. Use it to say “SiliconFlow sits at 5, OpenRouter at 7, Cursor at 8.” It is **not** an investment thesis: several boxes are whole supply chains; step 6 (the HTTP contract) is rarely a standalone company.

**Why it exists.** News and product launch posts show *one* SKU（库存单元） at a time. Beginners then treat “the model company” as the whole field. The sketch exists so you can park a new name on a step instead of treating every launch as a new universe.

**Mechanism.** Walk 1→9 as *who gets paid* and *what breaks*. Data (label / prefs / eval) is bought by the hour or the row. Train burns GPU-hour. “Get an LLM” is either a closed API card or an open weight file. Deploy is an engine + cluster (vLLM / SGLang), not a hosted product. Inference is a hosted token stream. API is the contract of that stream. Gateway holds keys, fallback, multi-model shopping. App is the user-facing product. User ≠ buyer.

**Boundaries.** Step 7 is **three jobs**, not one router product: [context layer](../part-03-context-router/ch-11-definition.md) = what history enters the window; [Model Router](../part-04-routing-nlu/ch-18-model-router.md) = which model; [Permission](../part-05-agent-runtime/ch-24-permission-identity.md) = which tools, as whom. Do not mash those three. Step 4 (engine) ≠ step 5 (hosted inference). Cursor can skip step 7 and call Anthropic/OpenAI directly; China relay / enterprise single-exit / multi-model shopping make the gateway mandatory.

**Example.** SiliconFlow wears engine + hosted inference + OpenAI-compat API as **one SKU**. AWS Bedrock is multi-model host + API + VPC/IAM — a cloud contract, not “just tokens.” Helicone / Portkey sell metering and traces; routing is the door, not the product.

**Failure modes.** Treating Scale AI as “all data” (they are labeling / prefs / eval, not Common Crawl). Treating “train” as one cost curve (pretrain and post-train are different — [Ch 4](./ch-04-train-and-posttrain.md)). Diagnosing a cache miss as “the gateway is dumb” when the app rewrote the prefix ([Ch 8](../part-02-inference-cost/ch-08-prompt-cache.md)).

## Comparison

| Step | Name | Their example | What it actually is | Bill | Diagnosis |
| --- | --- | --- | --- | --- | --- |
| 1 | Data | Scale AI | Labeling / prefs / eval — not all data | hour / row / token | Too narrow |
| 2 | Train | OpenAI / DeepSeek / Alibaba | Pretrain **and** post-train crushed | GPU-hour | Over-merged |
| 3 | Get an LLM | GPT / Claude / Qwen / Llama | Closed API **or** open weights | card / license / API | Usable |
| 4 | Deploy | vLLM / SGLang / TensorRT-LLM | Engine + cluster, not a hosted product | GPU rent | Mixes with 5 |
| 5 | Inference | SiliconFlow / Fireworks / Groq | Hosted token stream | in/out token | Real market |
| 6 | API | OpenAI-compatible HTTP | Contract of step 5 | RPM / TPM / $ | Rarely independent |
| 7 | Gateway / Router | OpenRouter / LiteLLM / 中转站 | Keys, fallback, multi-model | decision + markup | Optional but real |
| 8 | App | Cursor | User product; not only IDEs | seat / sub / markup | Too coarse |
| 9 | User | End user | Buyer ≠ user | time / outcome / seat | Usable |

**One SKU, three hats:**

| Firm | Wears | Buyer actually buys |
| --- | --- | --- |
| SiliconFlow（硅基流动） | engine + hosted inference + OpenAI-compat API (+ FT) | tokens, optional FT |
| AWS Bedrock | multi-model host + API + VPC/IAM | a cloud contract |
| Helicone / Portkey | gateway + traces + keys + invoice | metering/tracing; routing is the door |

Gaps: [Ch 2](./ch-02-refined-26.md).

**Read (读):**
- [docs — Scale AI’s official site: the labeling / prefs / eval firm that the 9-step sketch parks at step 1, not “all data.”](https://scale.com)
- [repo — vLLM: open-source high-throughput serving engine; this is step 4 (deploy), not a hosted SKU.](https://github.com/vllm-project/vllm)
- [repo — SGLang: serving runtime with its own scheduler and structured generation; another step-4 engine next to vLLM.](https://github.com/sgl-project/sglang)
- [docs — SiliconFlow: hosted inference + OpenAI-compat API as one SKU (steps 4+5+6).](https://www.siliconflow.com)
- [docs — Fireworks AI: hosted inference marketplace sitting at step 5.](https://fireworks.ai)
- [docs — Together AI: hosted inference plus fine-tune jobs; E2 and E3 in one vendor.](https://www.together.ai)
- [docs — OpenRouter: multi-model HTTP gateway (step 7) with unified billing.](https://openrouter.ai/docs)
- [repo — LiteLLM: self-host proxy that translates providers and does retry/fallback; gateway software, not a model.](https://github.com/BerriAI/litellm)

[Part I](./README.md) · [Edition](../README.md)
