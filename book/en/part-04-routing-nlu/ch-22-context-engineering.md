# Ch 22 · Context engineering（上下文工程）

Anthropic (2025-09): split this out of prompt engineering. The window is scarce working memory — orchestrate **all** of it, not just the system paragraph.

## Concept

**Definition.** Context engineering is the discipline of filling a finite window with the smallest high-signal token set: system, tools, skills metadata, retrieved docs, tool results, dialogue, memory files. Prompt engineering was the system paragraph. This is the rest of the working set.

**Why it exists.** Context rot: needle-in-haystack degrades with length even before the hard limit. Nominal 1M ≠ reliable 1M (Gemini CLI compacting at 50% is that admission). Prefix stability is a **cost** constraint (L0 → cache 0.1×). Lost-in-the-middle is a *position* failure, not a length failure.

**Mechanism.** Who writes which material (table). Levers: compact the full window vs surgically clear `tool_result` vs external memory vs just-in-time read via paths vs sub-agents that isolate a subtree. Skills: name+description in L0, body on demand.

**Boundaries.** Not model routing. Not relatedness. Not “buy 1M and stop thinking.” LangChain’s write/select/compress/isolate is a vocabulary, not a compiler SKU.

**Failure modes.** Duplicating tool docs in system. Schema bloat moving the prefix. Dirty memory writes (prompt injection). Treating cookbook three levers as one knob.

## Comparison — who writes which material

| Material | Writer | Typical failure |
| --- | --- | --- |
| System / developer | product | too long, duplicates tool docs |
| Tool schema | MCP / local tools | schema bloat → prefix moves → cache miss |
| Skill metadata | skills directory | put only name+description; body on demand |
| Retrieved docs | RAG / grep / read | lost-in-the-middle; duplicated slabs |
| Tool results | runtime | log bombs; Anthropic `clear_tool_uses` |
| Dialogue history | session | full window; compaction |
| Memory files | model-written | dirty writes, prompt injection |

**Read (读):**
- [docs — Anthropic Effective Context Engineering (2025-09): prompt vs context engineering; compaction / notes / sub-agents; just-in-time.](https://www.anthropic.com/engineering/effective-context-engineering-for-ai-agents)
- [docs — Claude cookbook: memory vs compact vs tool clearing as three APIs.](https://platform.claude.com/cookbook/tool-use-context-engineering-context-engineering-tools)
- [docs — Anthropic compaction.](https://platform.claude.com/docs/en/build-with-claude/compaction)
- [docs — Anthropic prompt caching: the 0.1× tax on an unstable prefix.](https://platform.claude.com/docs/en/build-with-claude/prompt-caching)
- [docs — OpenAI prompt caching.](https://developers.openai.com/api/docs/guides/prompt-caching)
- [docs — OpenAI compaction.](https://developers.openai.com/api/docs/guides/compaction)
- [paper — Lost in the Middle (Liu et al.): why “just buy 1M context” is not a strategy.](https://arxiv.org/abs/2307.03172)
- [repo — needle-in-a-haystack: recall vs length as an eval phenomenon.](https://github.com/gkamradt/LLMTest_NeedleInAHaystack)
- [docs — LangChain context-engineering: write / select / compress / isolate.](https://docs.langchain.com/oss/python/langchain/context-engineering)
- [paper — LongLLMLingua: query-aware compression when retrieved docs are the bloat.](https://arxiv.org/abs/2310.06839)

[Part IV](./README.md)
