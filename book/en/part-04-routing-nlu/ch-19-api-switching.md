# Ch 19 · API switching

Gateway problem. Intent is **not** “the router”.

## Concept

**Definition.** Translate dialects, pool connections, stick to a model while CONTINUE, fail over on 5xx/TPM. Before a vendor swap: project the window to **text transcript + open tool results**, rebuild L0.

**Why it exists.** OpenAI-compat is a dialect. Anthropic Messages, Gemini `contents`, Bedrock Converse do not share `tool_call`, system slot, or `cache_control`. Apps that “just POST JSON” discover this on the first failover.

**Mechanism.** Protocol layer rewrites fields. HTTP/2 pool, SSE, cancel — failover *before* first byte when possible. Stickiness: `session_id` → `model_id` until SHIFT / capability hole / 5xx, to protect cache + compact blob. Health: P90, TPM headroom, region, price. Fallback: LiteLLM / Portkey ordered chain. Keys: enterprise pool, fair RPM — the relay’s actual job.

**Cannot port:** OpenAI encrypted compact item, Anthropic cache hash, proprietary reasoning blocks, vendor-specific tool-result shapes.

**Boundaries.** Not NLU. Not task-aware compile. Not “auto picks the smartest model” unless you explicitly added [Ch 18](./ch-18-model-router.md).

**Failure modes.** Failing over mid-stream and keeping the old compact blob. Translating `cache_control` into a no-op and wondering why 0.1× vanished.

## Comparison

| Layer | Tech | Job |
| --- | --- | --- |
| Protocol | OpenAI-compat; Anthropic Messages ↔ Chat; Gemini contents | `tool_call`, system slot, `cache_control` don’t pass through |
| Connection | HTTP/2 pool, SSE, cancel | failover before first byte |
| Stickiness | `session_id` → `model_id` until SHIFT / capability hole / 5xx | protect cache + compact blob |
| Health | P90, TPM headroom, region, price | weighted random / watermarks |
| Fallback | LiteLLM / Portkey ordered chain | timeout → next; rewrite messages |
| Keys | enterprise key pool, fair RPM | the relay’s actual job |

**Read (读):**
- [docs — LiteLLM load balancing / fallback: ordered chains, not NLU.](https://docs.litellm.ai/docs/routing)
- [repo — Portkey-AI/gateway: dialect translate + fallback.](https://github.com/Portkey-AI/gateway)
- [docs — Anthropic Messages API: the dialect `cache_control` actually lives in.](https://platform.claude.com/docs/en/api/messages)
- [docs — OpenAI compaction: what dies on a vendor swap (encrypted item).](https://developers.openai.com/api/docs/guides/compaction)
- [docs — Anthropic compaction: readable block still vendor-preferring.](https://platform.claude.com/docs/en/build-with-claude/compaction)
- [docs — OpenRouter provider routing: how a hosted F3 picks an upstream.](https://openrouter.ai/docs/guides/routing/provider-selection)
- [docs — Gemini generateContent: a third body shape (`contents`), not Chat Completions.](https://ai.google.dev/api/generate-content)

[Part IV](./README.md)
