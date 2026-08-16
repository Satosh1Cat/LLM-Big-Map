# 第 5 章 · 评测门

「训完了」和「能卖 / 能买」之间缺的那一格。

## 概念

**定义。** 评测门是一个*决策*：出荷、采购、还是改 harness。不是体感，也不是一个 IQ 数。**Agent 分数 = 模型 + harness + 工具 + 提示 + 步数预算。** 换一块，分数就不可比。公开榜只覆盖很窄的任务组合。

**为什么存在。** 发布方需要出荷门。买方需要采购门。Agent 产品更需要，因为隐藏变量是脚手架（几乎只有 bash vs 带编辑工具，SWE-bench 能差几分到 >10 分——那是 [SOP](../part-05-agent-runtime/ch-26-coding-sop.md)，不是「智商」）。

**机制。** 选定任务族。冻结 harness。跑。按榜定义的指标报（fail-to-pass ∧ keep-pass；Elo；综合分）。Needle-in-a-haystack 是*评测现象*（召回 vs 长度），不要当核心论文。[Lost in the Middle](https://arxiv.org/abs/2307.03172) 才是长窗里的*位置*效应。离线评测该走 [Batch](../part-02-inference-cost/ch-09-batch-flex.md)（官方 50% / 约 24h），不是交互主循环。

**边界。** SWE-bench 量的是 issue → patch → 测试。τ-bench 量的是多轮工具 + 数据库终态。Arena 量的是人类偏好，不是代码。上下文层需要*自己的切片*（compact 之后还能续；换题之后旧约束不泄漏）——见 [第 25 章](../part-05-agent-runtime/ch-25-agent-evals.md)。不要拿 ATIS 意图准确率当 coding agent 的门。

**例子。** SWE-bench Verified 是 500 条人工筛过的 GitHub issue。mini-SWE-agent（几乎只有 bash）vs 带编辑工具的脚手架，是 harness 差，不是模型差。

**失败模式。** 刷榜。改了 prompt 却宣称新模型。把 needle-in-haystack 当成推翻了 Lost in the Middle。用 Flex/Batch 跑 agent eval 却不记录用的是哪个 SKU。

## 对比

| 榜 | 量什么 | 规模 | 分数 |
| --- | --- | --- | --- |
| SWE-bench Verified | GitHub issue → patch 过测试 | 500 人工筛 | fail-to-pass ∧ keep-pass |
| SWE-bench full / Lite | 同一族 | ~2294 / 300 | 同上 |
| LM Arena | 真人盲选偏好 | 活投票 | Elo |
| Artificial Analysis | 质量 + 速度 + 价格 | 托管端点 | 综合 |
| HELM | 多场景 | 场景集 | 分场景 |

**Read (读):**
- [paper — SWE-bench（Jimenez et al.）：真实 GitHub issue；单元测试当 oracle。](https://arxiv.org/abs/2310.06770)
- [repo — princeton-nlp/SWE-bench：数据、harness 与评测代码。](https://github.com/princeton-nlp/SWE-bench)
- [docs — swebench.com：Verified = 500 条人工审过；活榜。](https://www.swebench.com/)
- [paper — Lost in the Middle（Liu et al. 2023）：长上下文 U 形使用，中间最差。为什么「再买 1M」不够。](https://arxiv.org/abs/2307.03172)
- [repo — Kamradt needle-in-a-haystack：召回 vs 长度的评测现象，不是理论论文。](https://github.com/gkamradt/LLMTest_NeedleInAHaystack)
- [paper — HELM（Liang et al.）：多场景活评测，不是单一编程分数。](https://arxiv.org/abs/2211.09110)
- [docs — LMSYS Arena：真人盲选偏好 Elo。](https://lmarena.ai)
- [docs — Artificial Analysis：托管端点的质量 + 速度 + 价格综合。](https://artificialanalysis.ai)

[卷一](./README.md)
