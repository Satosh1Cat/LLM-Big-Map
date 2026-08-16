# Ch 11 · Definition & actions

Pick the smallest sufficient context for **this** task, with provenance（来源）, ACL, and valid time — then hand it to the target agent.

## Concept

**Definition.** The context layer decides *what history and files enter the window now*. Two product altitudes: **Pull** `search_context(...)` (easy, close to RAG) vs **Push** `compile(...)` before every model call (a Context Pack). “Router” here is easy to misread as *model* routing. It is not.

**Why it exists.** Windows are a finite attention budget (Anthropic, 2025-09). Dumping everything hits lost-in-the-middle and cache miss. Independent sellers of *task-aware compile* are still scarce; most apps bury G2 inside the product. Vendor **compaction APIs** fix a *full* window; they do not decide “should this history come along?”

**Mechanism — actions.** CONTINUE: same task, old window + new turn, prefix frozen → cache. COMPACT_AND_CONTINUE: same task, window full/noisy → history becomes summary/block + live tail. SELECTIVE_RETRIEVE: related, details in files → tool/read, don’t dump the corpus. RESET_PREFIX: task switched; old history is noise → drop dialogue body; keep system/tools/skills meta. FORK: related but parallel → new session copies pinned facts; no shared KV. HANDOFF_INGEST: foreign transcript → linearize → compact → pinned brief.

**Boundaries. Not:** [Model Router](../part-04-routing-nlu/ch-18-model-router.md) (which model). RAG shard picker. [Intent](../part-04-routing-nlu/ch-20-intent.md) (task type). Memory writers (Mem0/Zep/Letta) that persist notes *outside* the compile step. Three scores may coexist; do not fuse one “smart” softmax. Relatedness ≠ compacting ≠ intent ≠ model router.

**Priority sketch.** safety/ACL > current goal & explicit constraints > accepted/rejected decisions > artifacts > raw evidence > recent prose > high-level summary.

**Failure modes.** Compacting on every turn (cache dies). RESET that still inherits slots from the previous task. Treating MemGPT-style paging as a vendor compact blob.

## Comparison — CONTINUE vs SHIFT

| Action | Means | Window |
| --- | --- | --- |
| CONTINUE | same task | old window + new turn; prefix frozen → cache |
| COMPACT_AND_CONTINUE | same task, window full/noisy | history → summary/block + live tail |
| SELECTIVE_RETRIEVE | related, details in files | tool/read; don’t dump the corpus |
| RESET_PREFIX | task switched; old history is noise | drop dialogue body; keep system/tools/skills meta |
| FORK | related but parallel work | new session copies pinned facts; no shared KV |
| HANDOFF_INGEST | foreign transcript (ChatGPT, …) | linearize → compact → pinned brief |

**Read (读):**
- [docs — Anthropic Effective Context Engineering for AI Agents (2025-09): window = finite attention budget; smallest high-signal token set; compaction / notes / sub-agents; just-in-time retrieve. Does not define CONTINUE/RESET or ACL.](https://www.anthropic.com/engineering/effective-context-engineering-for-ai-agents)
- [docs — LangChain context engineering: write / select / compress / isolate vocabulary, not a compiler product.](https://docs.langchain.com/oss/python/langchain/context-engineering)
- [docs — Claude cookbook: memory vs full-window compact vs surgical `tool_result` clear — three different levers.](https://platform.claude.com/cookbook/tool-use-context-engineering-context-engineering-tools)
- [docs — Anthropic compaction API: vendor T2 on the *whole* transcript, not a relatedness gate.](https://platform.claude.com/docs/en/build-with-claude/compaction)
- [docs — OpenAI compaction: server-side compact; opaque item; still not “should this history come along?”](https://developers.openai.com/api/docs/guides/compaction)
- [paper — Lost in the Middle: why packing more tokens is not the same as packing the *right* tokens.](https://arxiv.org/abs/2307.03172)
- [paper — MemGPT (Packer et al.): OS-style paging of context; adjacent to G2 memory, different from vendor compact blobs.](https://arxiv.org/abs/2310.08560)
- [paper — Generative Agents (Park et al.): stream of memory + retrieval for simulated characters; not an API compact.](https://arxiv.org/abs/2304.03442)

[Part III](./README.md)
