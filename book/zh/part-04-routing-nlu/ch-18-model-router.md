# 第 18 章 · 任务 → Model Router

独立问题：**这一次打哪颗模型**。二元（强 vs 弱）或 k 路（改代码 / 长推理 / 闲聊 / 视觉 / 便宜分类）。

## 概念

**定义。** Router = 决定*哪颗模型*。Gateway = 密钥、协议翻译、traces、熔断（[第 7 章](../part-01-value-chain/ch-07-control-plane.md)，[第 19 章](./ch-19-api-switching.md)）。换模型会**打断** prompt cache 和 OpenAI 加密 compact blob。CONTINUE/SHIFT 从上下文层读；不要把两张网反融合。

**为什么存在。** 强模型贵。多数轮次用不上。RouteLLM / FrugalGPT 存在，是因为*给定成本下的质量*可以量，不是体感。

**机制。** 规则（token 数、代码围栏）0 ms。学习型（RouteLLM 在 Arena 偏好上的 MF / BERT / causal 分类器）10–40ms。级联（FrugalGPT）：先弱，质量分不够再升级——可能付第二次完整生成。ETH 2024：何时路由、何时级联最优；瓶颈是质量估计器。在线 bandit（BaRP / PILOT）移动策略。语义（vLLM Semantic Router）：查询 embedding → 表。

**边界。** 不是上下文打包。不是意图 NLU（虽然产品意图可以当*特征*）。LiteLLM fallback 链是*健康*路由，不是任务路由。OpenRouter `auto` 是托管路由器，优化的是*他们的*目标，不是你的。

**例子。** RouteLLM（LMSYS，ICLR 2025）：在*他们的*设定里，矩阵分解路由 ≈ **95% GPT-4 质量**，同时只把约 **14%** 请求打到强模。Arena/MT-Bench 数字——不是你的流量。

**失败模式。** CONTINUE 时换模型（cache 死）。把 Arena 路由器拿去打 SWE-bench 流量。每次都级联，弱+强都付钱。

## 对比

| 族 | 例子 | 何时 | 延迟税 |
| --- | --- | --- | --- |
| 规则 | token 数、代码围栏、plan、文件数 | 请求前 | 0 |
| 学习 | RouteLLM：Arena 偏好上的 MF / BERT / causal clf | 之前 | 10–40ms |
| 级联 | FrugalGPT：先弱，分不够再升级 | 可能第二次完整生成 | 一次弱生成 |
| 级联-路由 | ETH 2024：先路由再可能升级 | 混合 | 看质量估计器 |
| 在线 bandit | BaRP / PILOT | 之前；策略会动 | 0 额外前向 |
| 语义 | vLLM Semantic Router：查询 embed → 表 | 之前 | 一次 embed |

实现：LiteLLM（自托管 retry/fallback/预算）、OpenRouter `openrouter/auto`、Portkey、Not Diamond。

**Read (读):**
- [paper — RouteLLM（Ong et al.，ICLR 2025）：偏好数据路由器；*他们的榜*上约 95% GPT-4 质量、约 14% 打到强模。](https://arxiv.org/abs/2406.18665)
- [repo — lm-sys/RouteLLM：这些路由器的官方实现。](https://github.com/lm-sys/RouteLLM)
- [docs — LMSYS 博客 2024-07-01：RouteLLM 发布，以及 95%/14% 标题的上下文。](https://lmsys.org/blog/2024-07-01-routellm/)
- [paper — FrugalGPT（Chen et al. 2023）：级联——先便宜模型，再升级。](https://arxiv.org/abs/2305.05176)
- [paper — Routing and Cascading（Dekoninck et al.，ETH 2024）：何时各优；瓶颈是质量估计器。](https://arxiv.org/abs/2410.10347)
- [repo — vllm-project/semantic-router：按查询信号的 Mixture-of-Models 路由器，不是市场网关。](https://github.com/vllm-project/semantic-router)
- [repo — BerriAI/litellm：网关 fallback/预算；健康路由，不是任务路由。](https://github.com/BerriAI/litellm)
- [docs — OpenRouter：托管 `auto` 路由器 + 统一账单。](https://openrouter.ai/docs)
- [repo — Portkey-AI/gateway：带路由钩子的网关。](https://github.com/Portkey-AI/gateway)
- [repo — Helicone/helicone：挡在你所选路由器前面的可观测性门。](https://github.com/Helicone/helicone)

[卷四](./README.md)
