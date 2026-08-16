# 第 30 章 · ChatGPT handoff

Handoff 是 **HANDOFF_INGEST**，不是把树塞进 Cursor。ChatGPT 是消息树（分支、编辑、重新生成）。Agent 要的是**脊骨**。

## 概念

**定义。** 官方导出是一棵树：`mapping[id].parent / children`，`content.parts[]`。Agent 要线性化的脊骨，外加一份钉在 L1 的 T2 brief，不是可回放的 tool 轨迹——ChatGPT 的 tool-call 形状 ≠ Cursor 的。

**为什么存在。** 用户已经在 ChatGPT 里想过了。复制粘贴会丢掉分支、canvas、约束。过夜 Batch compact 可以；人盯着 ingest 不是 Batch 任务。

**机制。** 1 原始 JSON（导出或易碎的页内 API）。2 从 `current_node` 走到根（丢掉重新生成的旁支）。3 线性化 `role: user/assistant`，合并 parts，丢掉纯 UI。4 去噪（T0/T1）。5 整段历史当 T2 compact；可用 Batch。Brief：目标 / 约束 / 已定 / 未决 / 产物。6 把 brief 装进 L1 钉住。Cursor 类 session *从这里才开始*。

**边界。** 不是路径 3 去逛。不是续 ChatGPT 的 KV（你从来没有）。OpenAI 加密 compact item 仍然搬不过去——brief 必须是文本。

**失败模式。** 把树当 L3 活尾巴加载。把「谢谢」「再试一次」当约束留着。把 textdocs/canvas 当 tool 结果。

## 对比 — 树状导出 vs 线性 agent brief

| 步 | 工作 | 细节 |
| --- | --- | --- |
| 1 原始 JSON | 官方导出或页内 conversation API | 树：`mapping[id].parent / children`；`content.parts[]` |
| 2 脊骨 | 从 `current_node` 走到根 | 丢掉重新生成的旁支 |
| 3 线性化 | `role: user/assistant`，合并 parts，丢掉纯 UI | canvas/textdocs 另打包 |
| 4 去噪 | 丢掉「谢谢」/「再试一次」；留下决定、约束、代码、文件名 | T0/T1 |
| 5 Compact | 整段历史当 T2；可用 Batch | brief：目标 / 约束 / 已定 / 未决 / 产物 |
| 6 装载 | brief → L1 钉住 | Cursor 类 session *从这里才开始* |

**Read (读):**
- [docs — ChatGPT 导出帮助：ZIP / `conversations.json` 树，不是线性 API messages。](https://help.openai.com/en/articles/7260999-how-do-i-export-my-chatgpt-history-and-data)
- [docs — OpenAI compaction：在线性化脊骨上做 T2；离线用 Batch。](https://developers.openai.com/api/docs/guides/compaction)
- [docs — Anthropic compaction：若目标窗口是 Claude，用另一家 T2。](https://platform.claude.com/docs/en/build-with-claude/compaction)
- [docs — OpenAI Batch：过夜 ingest 走 50%/24h 队列。](https://platform.openai.com/docs/guides/batch)
- [docs — Anthropic 有效上下文工程：即时再读；brief 应指向文件，不要倒进去。](https://www.anthropic.com/engineering/effective-context-engineering-for-ai-agents)
- [paper — Lost in the Middle：为什么把整棵树塞进新窗口会失败。](https://arxiv.org/abs/2307.03172)

[卷六](./README.md)
