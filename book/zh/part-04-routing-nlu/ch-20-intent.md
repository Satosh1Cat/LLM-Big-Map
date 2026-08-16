# 第 20 章 · 意图识别

任务型 NLU 的左半：把一句话映射到标签表。**单句分类**，不是「这像不像这段历史」。

## 概念

**定义。** 意图是*你的*表里的一个标签：`code-edit` / `ask` / `search` / `browser` / `compact-now`，或 ATIS 的 `flight`。指标：Accuracy、Macro-F1、OOS 召回——和相关、槽位**分开报**。LLM 时代：小分类器、JSON 枚举，或把 `function_call.name` 当意图。

**为什么存在。** Agent 仍需要任务类型来挑工具、SKU 或模型族。经典数据集让你学*格式*（有没有 OOS），不是让你把航空标签拷进 coding agent。

**机制。** 经典：SVM+n-gram → BERT 句分类 → 联合意图-槽（Joint BERT、Slot-Gated）→ Rasa DIET。OOS：CLINC150 就是为拒识存在的。多意图：一句话两个标签。生产：function calling 让 `name` 当意图、arguments 当槽（[第 21 章](./ch-21-slot-filling.md)）。

**边界。** 不是换题（[第 15 章](../part-03-context-router/ch-15-relatedness.md)）。不是模型路由（虽然意图可以当路由器的*特征*）。产品表要**自己标**，不是 ATIS。

**失败模式。** 在 BANKING77 上训，部署到 GitHub issue。把意图和相关融进一个 softmax。没有拒识类，却把 OOS 当成「低置信意图」。

## 对比

| 块 | 内容 |
| --- | --- |
| 经典数据 | ATIS（航空）、SNIPS、CLINC150（OOS）、BANKING77、HWU64、MASSIVE（多语） |
| 经典模型 | SVM+n-gram → BERT 句分类 → 联合意图-槽（Joint BERT、Slot-Gated）→ Rasa DIET |
| OOS | 必须拒识；CLINC150 为此存在 |
| 多意图 | 一句话两个标签（「开灯并把空调调到 26」） |
| 路由表 | 不是对话 NLU——产品意图要自己标 |

**Read (读):**
- [paper — Joint BERT（Chen et al. 2019）：共享编码器，意图头 + 槽位头。不是换题。](https://arxiv.org/abs/1902.10909)
- [paper — ATIS（Hemphill et al. 1990）：航空出行话语；经典意图/槽格式，不是你的 coding-agent 标签。](https://aclanthology.org/H90-1021/)
- [paper — Snips NLU（Coucke et al. 2018）：众包语音意图；类别很少。](https://arxiv.org/abs/1805.10190)
- [paper — CLINC150（Larson et al. EMNLP 2019）：150 个域内意图 + 域外查询。](https://aclanthology.org/D19-1131/)
- [repo — clinc/oos-eval：CLINC150 数据文件。](https://github.com/clinc/oos-eval)
- [paper — BANKING77（Casanueva et al. 2020）：77 个细粒度银行意图，单域。](https://arxiv.org/abs/2003.04807)
- [repo — PolyAI-LDN/task-specific-datasets：BANKING77 数据发布。](https://github.com/PolyAI-LDN/task-specific-datasets)
- [docs — OpenAI function calling：生产里意图是 `name`，槽是 JSON。](https://platform.openai.com/docs/guides/function-calling)

[卷四](./README.md)
