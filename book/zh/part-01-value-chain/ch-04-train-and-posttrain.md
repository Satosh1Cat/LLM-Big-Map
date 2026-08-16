# 第 4 章 · 预训练与后训练

第 2 步把两条成本曲线压进一个词。

## 概念

**定义。** **预训练**是在 B1 规模原始 token 上做 next-token：超大集群、很少做一次，产出基座。**后训练**是在偏好、合成轨迹、可验证奖励上做 SFT / DPO / GRPO / RLVR / RLAIF：仍是 GPU-hour，量级小得多，真正把能用的模型出荷的节奏。安全 / 红队（C3）是*出荷门*，不是附录。蒸馏 / 开源 fork（C4）是*产品形态*，不是又一次从零训练。

**为什么存在。** 新闻里的「我们训了一个模型」分不清有人花了一次预训练，还是一个周末的 DPO。买方若不分这一刀，就分不清新基座和别人基座上的 LoRA。

**机制。** C1 读 B1，写出 checkpoint 或闭源 API。C2 读 B2/B3/B4-当奖励，写出对齐 / 会用工具 / 有领域腔的模型。托管 LoRA（E3）是架在别人 C1 上的*产品*——见 [第 6 章](./ch-06-inference-api.md)。应用不需要 C1。产品级拒答/风格可以放在 harness 里。

**边界。** 后训练不是「小一号的预训练」。RLVR（测试、数学）和 DPO 偏好对不是同一种监督。C3（METR、ASL、Preparedness）是*外部*门，不是训练配方。Hugging Face / ModelScope 上的开源权重是 C4/C5，不是托管推理。

**例子。** 实验室 system card 把「预训练配比」和「RLHF / constitutional」分开写，就是公开做这一章。Together / Fireworks 的 LoRA 任务是 E3，不是 C1。

**失败模式。** 把每个 checkpoint 都叫「新模型」。把安全当成发布后的博客。蒸馏教师，再拿教师泄漏过的评测打学生。

## 对比 — 预训练 vs 后训练

| | C1 预训练 | C2 后训练 |
| --- | --- | --- |
| 输入 | B1 原始 token | B2 偏好、B3 合成、B4 当 RLVR |
| 账单 | 集群规模 GPU-hour | 仍是 GPU-hour，小得多；或托管 FT |
| 方法 | 网页/代码 next-token | SFT, DPO, GRPO, RLVR, RLAIF |
| 谁 | 前沿实验室 | 实验室；Together 等也卖 FT |
| 节奏 | 稀、大 | 勤，出荷能用的模型 |

| 方法 | 监督 | 典型用途 |
| --- | --- | --- |
| SFT | 示范 | 格式、工具、领域腔 |
| DPO | 偏好对 | 不另训奖励模型 |
| GRPO / RLVR | 可验证奖励（测试、数学） | 编程 / 推理 |
| RLAIF | 模型写的偏好 | 比 B2 便宜 |

**Read (读):**
- [paper — Attention Is All You Need（Vaswani et al.）：每个 C1 仍坐在上面的 Transformer 基线。](https://arxiv.org/abs/1706.03762)
- [paper — InstructGPT（Ouyang et al.）：公开后训练配方——先 SFT 再从偏好做 PPO；是 C2，不是 C1。](https://arxiv.org/abs/2203.02155)
- [paper — Direct Preference Optimization（Rafailov et al.）：DPO，偏好对、不另训奖励模型。](https://arxiv.org/abs/2305.18290)
- [paper — DeepSeek-R1：大规模推理后训练 + RL；是 C2 叙事，不是芯片论文。](https://arxiv.org/abs/2501.12948)
- [docs — METR：独立的 C3 自主能力评估，不是训练配方。](https://metr.org)
- [docs — Anthropic Claude system card（当前入口）：一家实验室如何公开拆预训练 / 后训练 / 安全。](https://www.anthropic.com/claude)
- [docs — Hugging Face Hub：C4/C5 权重分发，不是托管 token 表。](https://huggingface.co/models)

[卷一](./README.md)
