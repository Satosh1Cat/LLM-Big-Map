# 第 15 章 · 查询–历史相关

篇章切分：这句话还是不是**同一条对话线**？不是意图分类。

## 概念

**定义。** 相关问的是新话是否延续历史的*同一条线*（CONTINUE），还是已经切走（SHIFT / NEW）。回顾式：看到下一轮再标切题（TIAGE）。预测式：只有历史——*下一轮*会不会漂？这是篇章，不是 NLU 意图。

**为什么存在。** 上下文层第 1 阶段只要这一个标签。意图可以一直是「写代码」，题目已经从鉴权跳到 CSS。意图可以变，实体还在（同一个文件，「测试为什么挂」）。若把「重构 vs 问答」和换题放进同一个 softmax，RESET 和 CONTINUE 会一起错。

**机制。** 特征：回指（「继续」「那个函数」）、新实体（没见过的路径）、时间间隔、代码符号 Jaccard、任务陈述 NLI（gist 当假设 → 蕴含 / 矛盾 / 中性）、用户明示（「新话题」）。判定器：词汇 / embedding / NLI / topic-shift / 实体连续。热路径应 <20ms；再上一个 LLM 分类器是成本，不是默认。

**边界。** 不是 [意图](../part-04-routing-nlu/ch-20-intent.md)。不是 compacting。不是模型路由。TextTiling / C99 / LCSeg 是无监督*线性*切分词汇——短聊天上弱，但仍是「边界」这套词。

**例子。** TIAGE 在 PersonaChat 上标了 7861 条黄金 topic-shift，并拆检测 vs 生成。中文换题论文（如 2305.01195）仍然是切题，仍然不是 BANKING77 意图。

**失败模式。** 用 ATIS 意图训相关。RESET 了还继承 slot。把 embedding 余弦 0.7 当成宇宙 SHIFT 阈值。

## 对比

| 设定 | 定义 | 文献 |
| --- | --- | --- |
| 回顾式 | 看到下一轮再标切题 | TIAGE（EMNLP 2021） |
| 预测式 | 只有历史，*下一轮*会不会漂？ | TIAGE TSManager |
| 主题切分 | 长流上的边界 | TextTiling, C99, LCSeg, BERT-Wiki727k |
| 无监督聊天 | embed + 时间间隔 + 说话人 → 聚类 | 2025 聊天切题论文 |

| 特征 | 例子 |
| --- | --- |
| 回指 | 「继续」「那个函数」→ CONTINUE 先前 |
| 新实体 | 没见过的路径 / URL / 产品 |
| 时间间隔 | 同一 session，数小时后 → 更高切题先验 |
| 代码符号重叠 | 标识符 Jaccard |
| 任务陈述 NLI | gist 当假设 → 蕴含 / 矛盾 / 中性 |
| 用户明示 | 「新话题」「算了，改做 X」 |

**Read (读):**
- [paper — TIAGE（Xie et al.，EMNLP 2021）：PersonaChat 上的 topic-shift 数据；回顾 vs 预测。不是意图表。](https://arxiv.org/abs/2109.04562)
- [paper — 中文对话 Topic Shift Detection：中文切题标签；仍不是 NLU 意图。](https://arxiv.org/abs/2305.01195)
- [paper — TextTiling（Hearst 1997）：经典无监督线性主题切分。](https://aclanthology.org/J97-1003/)
- [paper — C99（Choi）：另一经典线性切分器；「边界」词汇。](https://aclanthology.org/C00-1070/)
- [paper — Text Segmentation as a Supervised Learning Task（Koshorek et al.）：Wiki-727K 长流神经边界，不是聊天意图。](https://arxiv.org/abs/1803.05355)
- [paper — MNLI（Williams et al.）：任务陈述 NLI 借用的蕴含/矛盾/中性标签。](https://arxiv.org/abs/1704.05426)

[卷三](./README.md)
