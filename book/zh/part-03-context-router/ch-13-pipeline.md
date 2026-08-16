# 第 13 章 · 典型管线

多数生产系统会重新发现的六个阶段。不是产品规格——是依赖顺序。

## 概念

**定义。** 观测 → 相关门 → compact → 组装 L0–L4 → 模型门 → 外来 ingest。顺序是*依赖*：没有第 0 阶段日志 → 没有相关训练集。没有相关门 → 每轮 compact → cache 死。跳过 T0/T1 修剪直接 LLM 摘要 → 账单和延迟先爆。Mamba（T3）替代不了厂商 T2 API。

**为什么存在。** 团队往往在第一次 cache miss 事故之后才发明这套管线。把阶段命名清楚，是为了让相关、compacting、模型路由继续当分开的过程。

**机制。** 第 0 阶段记 token、cache hit、换题、compact、换模型。第 1 阶段相关（embed / 小分类器，<20ms）吐出 CONTINUE / SHIFT / NEW。第 2 阶段 compact：T0 修剪 → T1 抽取 → T2 摘要（厂商/LLM）→ 也许 T3 SSM。第 3 阶段按 L0 冻结打包 L0–L4。第 4 阶段只在 SHIFT/NEW 或能力/预算缺口时换模型；CONTINUE 黏住。第 5 阶段把 ChatGPT/网页 transcript 线性化成 HANDOFF 包（离线可用 Batch）。

**边界。** 相关判定留在 [第 15 章](./ch-15-relatedness.md)。Compact 层级留在 [第 14 章](./ch-14-compacting.md)。模型门是 [第 18 章](../part-04-routing-nlu/ch-18-model-router.md)，不是本文件。不要把 cookbook 的「memory / compact / 清 tool」当成一个旋钮。

**失败模式。** 相关门还没过就跑 T2 compact。每轮用意图标签组装 L0。把 ChatGPT 树当成 API 线性 `messages` ingest。

## 对比

| 阶段 | 工作 | 入 | 出 | 跑在 |
| --- | --- | --- | --- | --- |
| 0 观测 | 记 token、cache hit、换题、compact、换模型 | 每个请求 | traces | Gateway |
| 1 相关门 | CONTINUE / SHIFT / NEW | 查询 + 尾巴 + 可选 gist | 动作 + 分数 | embed / 小分类器，<20ms |
| 2 Compact | T0 修剪 → T1 抽取 → T2 摘要 → 也许 T3 SSM | 历史条目 | 新窗口 + 记录 | 客户端或厂商 compact API |
| 3 组装 | 打包 L0–L4；L0 字节稳定 | 动作 + 材料 | `messages[]` | 路由进程 |
| 4 模型门 | 只在 SHIFT/NEW 或能力/预算缺口时换；CONTINUE 黏住 | 任务类型 + 健康 | 厂商调用 | Gateway |
| 5 外来 ingest | ChatGPT/网页 transcript → 脊骨 → compact → 钉住 brief | 导出 JSON | HANDOFF 包 | 离线可用 Batch |

**Read (读):**
- [docs — Claude cookbook：memory / compaction / 清 tool 是三件*不同*杠杆，不是一个管线阶段。](https://platform.claude.com/cookbook/tool-use-context-engineering-context-engineering-tools)
- [docs — Anthropic 有效上下文工程：按路径即时再读；子 agent 当隔离。](https://www.anthropic.com/engineering/effective-context-engineering-for-ai-agents)
- [docs — Anthropic compaction：本管线可能调用的 T2 厂商 API，不是相关门。](https://platform.claude.com/docs/en/build-with-claude/compaction)
- [docs — OpenAI `POST /responses/compact` 参考：独立 compact 方法。](https://developers.openai.com/api/reference/resources/responses/methods/compact)
- [docs — Helicone tracing：第 0 阶段在网关里实际记什么。](https://docs.helicone.ai)
- [paper — TIAGE：第 1 阶段*可以*拿来学的 topic-shift 标签；不是意图。](https://arxiv.org/abs/2109.04562)
- [docs — LangSmith：存放本管线需要的第 0 阶段 traces 的另一处 F2。](https://docs.smith.langchain.com)

[卷三](./README.md)
