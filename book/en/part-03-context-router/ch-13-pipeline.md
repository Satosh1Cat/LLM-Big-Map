# Ch 13 · A typical pipeline

Six stages most production systems rediscover. Not a product spec — a dependency order.

## Concept

**Definition.** Observe → relatedness gate → compact → assemble L0–L4 → model gate → foreign ingest. The order is a *dependency*: no stage-0 logs → no relatedness training set. No relatedness gate → compact every turn → cache dies. Skip T0/T1 prune → jump to LLM summary → bill and latency explode first. Mamba (T3) does not replace vendor T2 APIs.

**Why it exists.** Teams reinvent this after the first cache-miss incident. The pipeline names the stages so relatedness, compacting, and model routing stay separate processes.

**Mechanism.** Stage 0 logs tokens, cache hit, topic shift, compact, model swap. Stage 1 relatedness (embed / tiny clf, <20ms) emits CONTINUE / SHIFT / NEW. Stage 2 compact: T0 prune → T1 extract → T2 abstract (vendor/LLM) → maybe T3 SSM. Stage 3 packs L0–L4 with L0 frozen. Stage 4 swaps model only on SHIFT/NEW or a capability/budget hole; sticks on CONTINUE. Stage 5 linearizes a ChatGPT/web transcript into a HANDOFF pack (Batch OK offline).

**Boundaries.** Relatedness judges stay in [Ch 15](./ch-15-relatedness.md). Compact levels stay in [Ch 14](./ch-14-compacting.md). Model gate is [Ch 18](../part-04-routing-nlu/ch-18-model-router.md), not this file. Do not treat cookbook “memory / compact / tool clear” as one knob.

**Failure modes.** Running T2 compact before the relatedness gate. Assembling L0 from intent labels every turn. Ingesting a ChatGPT tree as if it were API-linear `messages`.

## Comparison

| Stage | Job | In | Out | Runs on |
| --- | --- | --- | --- | --- |
| 0 Observe | log tokens, cache hit, topic shift, compact, model swap | every request | traces | gateway |
| 1 Relatedness gate | CONTINUE / SHIFT / NEW | query + tail + optional gist | action + score | embed / tiny clf, <20ms |
| 2 Compact | T0 prune → T1 extract → T2 abstract → maybe T3 SSM | history items | new window + record | client or vendor compact API |
| 3 Assemble | pack L0–L4; keep L0 byte-stable | action + materials | `messages[]` | router process |
| 4 Model gate | swap only on SHIFT/NEW or capability/budget hole; stick on CONTINUE | task type + health | provider call | gateway |
| 5 Foreign ingest | ChatGPT/web transcript → spine → compact → pinned brief | export JSON | HANDOFF pack | Batch OK offline |

**Read (读):**
- [docs — Claude cookbook memory / compaction / tool clearing: three *different* levers, not one pipeline stage.](https://platform.claude.com/cookbook/tool-use-context-engineering-context-engineering-tools)
- [docs — Anthropic effective context engineering: just-in-time retrieve via paths; sub-agents as isolation.](https://www.anthropic.com/engineering/effective-context-engineering-for-ai-agents)
- [docs — Anthropic compaction: the T2 vendor API this pipeline may call, not the relatedness gate.](https://platform.claude.com/docs/en/build-with-claude/compaction)
- [docs — OpenAI `POST /responses/compact` reference: standalone compact method.](https://developers.openai.com/api/reference/resources/responses/methods/compact)
- [docs — Helicone tracing: what stage 0 actually records in a gateway.](https://docs.helicone.ai)
- [paper — TIAGE: topic-shift labels that stage 1 is *allowed* to learn from; not intent.](https://arxiv.org/abs/2109.04562)
- [docs — LangSmith: another F2 place to store the stage-0 traces this pipeline needs.](https://docs.smith.langchain.com)

[Part III](./README.md)
