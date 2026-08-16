# 第 11 章 · 定义与动作

为**这一次**任务挑最小够用的上下文，带 provenance（来源）、ACL、有效时间——再交给目标 agent。

## 概念

**定义。** 上下文层决定*此刻哪些历史和文件进窗口*。两种产品高度：**Pull** `search_context(...)`（容易，接近 RAG）vs **Push** 每次模型调用前 `compile(...)`（一份 Context Pack）。这里的「Router」很容易被读成*模型*路由。不是。

**为什么存在。** 窗口是有限注意力预算（Anthropic，2025-09）。全倒进去会撞 lost-in-the-middle 和 cache miss。*任务感知 compile* 的独立卖方仍然少；多数应用把 G2 埋在产品里。厂商 **compaction API** 修的是*整窗*；它们不决定「这段历史该不该跟着走」。

**机制 — 动作。** CONTINUE：同一任务，旧窗 + 新一轮，前缀冻结 → cache。COMPACT_AND_CONTINUE：同一任务，窗口满/吵 → 历史变成摘要/块 + 活尾巴。SELECTIVE_RETRIEVE：相关，细节在文件里 → tool/read，不要把语料库倒进去。RESET_PREFIX：任务切了，旧历史是噪声 → 丢掉对话正文；保留 system/tools/skills 元数据。FORK：相关但并行 → 新 session 拷钉住的事实；不共享 KV。HANDOFF_INGEST：外来 transcript → 线性化 → compact → 钉住的 brief。

**边界。不是：** [Model Router](../part-04-routing-nlu/ch-18-model-router.md)（哪颗模型）。RAG 分片选择器。[意图](../part-04-routing-nlu/ch-20-intent.md)（任务类型）。把笔记写在 compile 之外的 memory 写入器（Mem0/Zep/Letta）。三个分数可以并存；不要融成一个「聪明」softmax。相关 ≠ compacting ≠ 意图 ≠ 模型路由。

**优先级草图。** 安全/ACL > 当前目标与显式约束 > 已接受/已拒绝的决定 > 产物 > 原始证据 > 近处散文 > 高层摘要。

**失败模式。** 每轮都 compact（cache 死）。RESET 了却还从上一任务继承 slot。把 MemGPT 式分页当成厂商 compact blob。

## 对比 — CONTINUE vs SHIFT

| 动作 | 含义 | 窗口 |
| --- | --- | --- |
| CONTINUE | 同一任务 | 旧窗 + 新一轮；前缀冻结 → cache |
| COMPACT_AND_CONTINUE | 同一任务，窗满/吵 | 历史 → 摘要/块 + 活尾巴 |
| SELECTIVE_RETRIEVE | 相关，细节在文件 | tool/read；不要倒语料库 |
| RESET_PREFIX | 任务切了；旧历史是噪声 | 丢掉对话正文；留 system/tools/skills 元数据 |
| FORK | 相关但并行 | 新 session 拷钉住的事实；不共享 KV |
| HANDOFF_INGEST | 外来 transcript（ChatGPT …） | 线性化 → compact → 钉住的 brief |

**Read (读):**
- [docs — Anthropic Effective Context Engineering for AI Agents（2025-09）：窗口 = 有限注意力预算；最小高信号 token 集；compaction / 笔记 / 子 agent；即时再读。不定义 CONTINUE/RESET，也不管 ACL。](https://www.anthropic.com/engineering/effective-context-engineering-for-ai-agents)
- [docs — LangChain 上下文工程：write / select / compress / isolate 词汇，不是编译器产品。](https://docs.langchain.com/oss/python/langchain/context-engineering)
- [docs — Claude cookbook：memory vs 整窗 compact vs 外科手术清 `tool_result`——三件不同杠杆。](https://platform.claude.com/cookbook/tool-use-context-engineering-context-engineering-tools)
- [docs — Anthropic compaction API：厂商 T2 打在*整段* transcript 上，不是相关门。](https://platform.claude.com/docs/en/build-with-claude/compaction)
- [docs — OpenAI compaction：服务端 compact；不透明 item；仍然不是「这段历史该不该跟着」。](https://developers.openai.com/api/docs/guides/compaction)
- [paper — Lost in the Middle：为什么多塞 token ≠ 塞对 token。](https://arxiv.org/abs/2307.03172)
- [paper — MemGPT（Packer et al.）：操作系统式的上下文分页；靠近 G2 记忆，不同于厂商 compact blob。](https://arxiv.org/abs/2310.08560)
- [paper — Generative Agents（Park et al.）：记忆流 + 检索的模拟角色；不是 API compact。](https://arxiv.org/abs/2304.03442)

[卷三](./README.md)
