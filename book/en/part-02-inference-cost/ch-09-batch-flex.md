# Ch 9 · Batch & Flex

Trade **latency** for **capacity fill**. Official list price, not a side deal.

## Concept

**Definition.** **Batch** is an offline queue: submit JSONL, provider fills the trough, completion window **~24h**. OpenAI and Anthropic list **50% off input and output** vs the synchronous API. **Flex** is still the **realtime** API (`service_tier=flex`) at ~50% token discount, with higher latency. Neither is a cracked key.

**Why it exists.** GPUs have idle troughs. Labs would rather sell them cheaper than leave them empty. Agent main loops (a human staring at a cursor) cannot wait 24h. Offline evals, embedding backfill, and night SOP distill can.

**Mechanism — Batch.** `POST /batches` (OpenAI) or Anthropic Message Batches. JSONL rows with unique `custom_id`. Separate rate limits from the realtime pool. Prompt cache *can* stack, but shards/TTL are less stable than a sticky interactive session. Results come as another JSONL.

**Mechanism — Flex.** Same Responses / Chat URL, `service_tier=flex`. Streaming and `prompt_cache_key` still work because it is still realtime. Latency is worse than default; price is the point.

**Boundaries.** Batch ≠ Flex. Flex ≠ prompt cache (cache is a prefix discount *on* whatever tier you already chose). Batch is the right queue for [evals](../part-05-agent-runtime/ch-25-agent-evals.md) and [auto-SOP](../part-05-agent-runtime/ch-27-skills-auto-sop.md), not for [ChatGPT ingest](../part-06-ingress-market/ch-30-chatgpt-handoff.md) if a human is waiting — unless the ingest is overnight.

**Example.** Cookbook anecdote (not *your* traffic): same 10k repeated requests, Flex+extended cache beat Batch cache hit by ~8.5% and input cost ~23% lower — because Flex keeps realtime cache semantics. Use that as a hypothesis, then measure.

**Failure modes.** Putting the agent loop on Batch. Comparing Batch 50% to a Flex 50% *plus* cache 0.1× as if they were one multiplier. Forgetting `custom_id` and being unable to join results.

## Comparison — Batch 50%/24h vs Flex 50% realtime

| | Interactive | Batch | Flex |
| --- | --- | --- | --- |
| Latency | seconds | minutes–24h | slower than default realtime |
| Price | 1.0× | **0.5× in + 0.5× out** | ~50% token discount |
| Cache | full prompt-cache semantics | can stack; shards/TTL less stable | realtime semantics |
| Protocol | `/chat/completions`, `/messages`, `/responses` | `POST /batches` + JSONL | Responses + `service_tier=flex` |
| Use | agent loop | evals, embeddings backfill, night SOP distill | tolerant quasi-realtime |

**Read (读):**
- [docs — OpenAI Batch API: official **50%** vs sync, 24h window, separate rate limits, JSONL + `custom_id`.](https://platform.openai.com/docs/guides/batch)
- [docs — Anthropic Message Batches: official 50% batch pricing and 24h processing for Messages.](https://platform.claude.com/docs/en/build-with-claude/batch-processing)
- [docs — OpenAI Flex processing: ~50% on the realtime API via `service_tier=flex`; not a 24h queue.](https://platform.openai.com/docs/guides/flex-processing)
- [docs — OpenAI Prompt Caching 201: Flex vs Batch when cache hits dominate the bill.](https://developers.openai.com/cookbook/examples/prompt_caching_201)
- [docs — OpenAI prompt caching: the prefix discount that stacks (or fails to) on both SKUs.](https://developers.openai.com/api/docs/guides/prompt-caching)
- [docs — Anthropic prompt caching: **0.1×** read still applies when the Batch/interactive prefix is stable.](https://platform.claude.com/docs/en/build-with-claude/prompt-caching)

[Part II](./README.md)
