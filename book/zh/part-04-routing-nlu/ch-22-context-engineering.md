# 第 22 章 · 上下文工程

Anthropic（2025-09）：从提示工程里拆出来。窗口是稀缺工作记忆——编排**全部**，不只是 system 那段。

## 概念

**定义。** 上下文工程是往有限窗口里填最小高信号 token 集的纪律：system、工具、skills 元数据、检索文档、tool 结果、对话、记忆文件。提示工程管的是 system 段。这里管的是其余工作集。

**为什么存在。** Context rot：needle-in-haystack 随长度变差，还没碰到硬上限。名义 1M ≠ 可靠 1M（Gemini CLI 在 50% compact 就是承认这一点）。前缀稳定是**成本**约束（L0 → cache 0.1×）。Lost-in-the-middle 是*位置*失败，不是长度失败。

**机制。** 谁写哪类材料（见表）。杠杆：整窗 compact vs 外科手术清 `tool_result` vs 窗外 memory vs 按路径即时再读 vs 用子 agent 隔离一棵子树。Skills：L0 只放 name+description，正文按需。

**边界。** 不是模型路由。不是相关。不是「买 1M 就不用想」。LangChain 的 write/select/compress/isolate 是词汇，不是编译器 SKU。

**失败模式。** 在 system 里重复工具文档。Schema 膨胀推动前缀。脏记忆写入（提示注入）。把 cookbook 三件杠杆当成一个旋钮。

## 对比 — 谁写哪类材料

| 材料 | 作者 | 典型失败 |
| --- | --- | --- |
| System / developer | 产品 | 太长，重复工具文档 |
| Tool schema | MCP / 本地工具 | schema 膨胀 → 前缀移动 → cache miss |
| Skill 元数据 | skills 目录 | 只放 name+description；正文按需 |
| 检索文档 | RAG / grep / read | lost-in-the-middle；重复厚板 |
| Tool 结果 | 运行时 | 日志炸弹；Anthropic `clear_tool_uses` |
| 对话历史 | session | 整窗；compaction |
| 记忆文件 | 模型写 | 脏写、提示注入 |

**Read (读):**
- [docs — Anthropic Effective Context Engineering（2025-09）：提示工程 vs 上下文工程；compaction / 笔记 / 子 agent；即时再读。](https://www.anthropic.com/engineering/effective-context-engineering-for-ai-agents)
- [docs — Claude cookbook：memory vs compact vs 清 tool 作为三套 API。](https://platform.claude.com/cookbook/tool-use-context-engineering-context-engineering-tools)
- [docs — Anthropic compaction。](https://platform.claude.com/docs/en/build-with-claude/compaction)
- [docs — Anthropic prompt caching：不稳定前缀上的 0.1× 税。](https://platform.claude.com/docs/en/build-with-claude/prompt-caching)
- [docs — OpenAI prompt caching。](https://developers.openai.com/api/docs/guides/prompt-caching)
- [docs — OpenAI compaction。](https://developers.openai.com/api/docs/guides/compaction)
- [paper — Lost in the Middle（Liu et al.）：为什么「再买 1M」不是策略。](https://arxiv.org/abs/2307.03172)
- [repo — needle-in-a-haystack：召回 vs 长度的评测现象。](https://github.com/gkamradt/LLMTest_NeedleInAHaystack)
- [docs — LangChain 上下文工程：write / select / compress / isolate。](https://docs.langchain.com/oss/python/langchain/context-engineering)
- [paper — LongLLMLingua：检索文档把窗口撑爆时的 query-aware 压缩。](https://arxiv.org/abs/2310.06839)

[卷四](./README.md)
