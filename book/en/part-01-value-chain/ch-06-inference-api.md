# Ch 6 · Inference & API

Turn weights into a token stream, then into HTTP. Org chart: 4 → 5 → 6. Price list: often **one SKU**.

## Concept

**Definition.** **E1 engine** runs the model on GPUs you rent. **E2 hosted inference** sells a token stream (SiliconFlow / Fireworks / Groq / Bedrock). **E5** is the HTTP contract of that stream — Completions, Responses, Messages, Converse — rarely a company. **E3** is hosted fine-tune / LoRA as a product. **E4** (Cache / Batch / Flex) sits on the same stream as extra SKUs — [Part II](../part-02-inference-cost/README.md).

**Why it exists.** Buyers meet “an API.” Engineering has to know whether they bought an engine, a host, a contract dialect, or a discount SKU. OpenAI-compatible is a *de-facto dialect*, not a law: `tool_call`, `system` placement, `cache_control` do not pass through.

**Mechanism.** Self-host: GPU + vLLM/SGLang, you *are* E1. Buy SiliconFlow / Fireworks / Bedrock: E1 disappears inside E2; you pay in/out tokens under an E5 JSON dialect. Fine-tune jobs (E3) write adapters that still serve through E2. Pricing SKUs (E4) multiply the same tokens: Anthropic cache read **0.1×** input; OpenAI Batch **0.5× in + 0.5× out**; Flex ~50% on the realtime API.

**Boundaries.** Engine ≠ hoster. Hoster ≠ gateway (F3). API contract ≠ marketplace. A model swap is an E5 dialect change *and* a cache miss *and* death of encrypted compact blobs.

**Example.** SiliconFlow as 4+5+6 in one SKU: engine eaten, hosted tokens billed, OpenAI-compat JSON. Bedrock Converse is a *different* contract from Anthropic Messages — same “chat,” not the same fields.

**Failure modes.** Assuming `cache_control` survives LiteLLM translation. Comparing Fireworks tokens to vLLM GPU-hour as if they were one unit. Using Batch for the interactive cursor loop.

## Comparison — SiliconFlow as 4+5+6

| Hat | Layer | In the SKU? |
| --- | --- | --- |
| Engine | E1 | eaten by the hoster |
| Hosted tokens | E2 | what you pay |
| HTTP contract | E5 | OpenAI-compat JSON |
| Fine-tune | E3 | optional extra |

Self-host: you buy GPU + vLLM/SGLang. Buy Bedrock / SiliconFlow: E1 disappears.

| SKU | vs realtime 1.0× | Latency |
| --- | --- | --- |
| Realtime | 1.0× | seconds |
| Cache read | Anthropic **0.1×** input; OpenAI 0.5×–0.1× | realtime |
| Batch | **0.5× in + 0.5× out** | minutes–~24h |
| Flex | ~50% token discount, still realtime API | slower than default |

**Read (读):**
- [docs — OpenAI API reference: Chat Completions / Responses — one E5 dialect.](https://platform.openai.com/docs/api-reference)
- [docs — Anthropic Messages API: a second E5 dialect; `cache_control` lives here.](https://platform.claude.com/docs/en/api/messages)
- [docs — AWS Bedrock Converse: a third contract (not OpenAI-compat by default).](https://docs.aws.amazon.com/bedrock/latest/userguide/conversation-inference.html)
- [repo — vLLM: E1 engine you run if you did not buy E2.](https://github.com/vllm-project/vllm)
- [repo — SGLang: E1 engine with its own runtime and structured generation.](https://github.com/sgl-project/sglang)
- [docs — SiliconFlow: hosted E2+E5 SKU.](https://www.siliconflow.com)
- [docs — Fireworks AI: hosted inference at E2.](https://fireworks.ai)
- [docs — Together AI: hosted inference + FT jobs (E2+E3).](https://www.together.ai)
- [docs — Anthropic prompt caching: official **0.1×** cache-read input (E4).](https://platform.claude.com/docs/en/build-with-claude/prompt-caching)

[Part I](./README.md)
