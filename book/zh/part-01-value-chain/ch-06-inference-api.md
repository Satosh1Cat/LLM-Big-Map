# 第 6 章 · 推理与 API

把权重变成 token 流，再变成 HTTP。组织图：4 → 5 → 6。价目表：常常是 **一个 SKU**。

## 概念

**定义。** **E1 引擎**在你租的 GPU 上跑模型。**E2 托管推理**卖 token 流（硅基流动 / Fireworks / Groq / Bedrock）。**E5** 是这条流的 HTTP 契约——Completions、Responses、Messages、Converse——很少是一家公司。**E3** 是托管微调 / LoRA 产品。**E4**（Cache / Batch / Flex）是同一条流上的额外 SKU——[卷二](../part-02-inference-cost/README.md)。

**为什么存在。** 买方碰到的是「一个 API」。工程必须知道买到的是引擎、托管、契约方言，还是折扣 SKU。OpenAI-compatible 是*事实方言*，不是法律：`tool_call`、`system` 位置、`cache_control` 过不了网关就丢。

**机制。** 自托管：GPU + vLLM/SGLang，你*就是* E1。买硅基流动 / Fireworks / Bedrock：E1 消失在 E2 里；按 E5 JSON 方言付输入/输出 token。微调任务（E3）写出的 adapter 仍经 E2 提供服务。计价 SKU（E4）乘在同一批 token 上：Anthropic cache read **0.1×** input；OpenAI Batch **0.5× in + 0.5× out**；Flex 在实时 API 上大约 50%。

**边界。** 引擎 ≠ 托管商。托管商 ≠ Gateway（F3）。API 契约 ≠ 市场。换模型是 E5 方言变化，*并且* cache miss，*并且*加密 compact blob 作废。

**例子。** 硅基流动把 4+5+6 做成一个 SKU：引擎被吃掉，托管 token 计费，OpenAI 兼容 JSON。Bedrock Converse 和 Anthropic Messages 是*不同*契约——都叫「聊天」，字段不是同一套。

**失败模式。** 以为 `cache_control` 能活过 LiteLLM 翻译。把 Fireworks 的 token 和 vLLM 的 GPU-hour 当同一个单位比。把 Batch 用在用户盯着光标的主循环上。

## 对比 — 硅基流动作为 4+5+6

| 帽子 | 层 | 在 SKU 里吗？ |
| --- | --- | --- |
| 引擎 | E1 | 被托管商吃掉 |
| 托管 token | E2 | 你付的钱 |
| HTTP 契约 | E5 | OpenAI 兼容 JSON |
| 微调 | E3 | 可选附加 |

自托管：买 GPU + vLLM/SGLang。买 Bedrock / 硅基流动：E1 消失。

| SKU | 相对实时 1.0× | 延迟 |
| --- | --- | --- |
| 实时 | 1.0× | 秒 |
| Cache read | Anthropic **0.1×** input；OpenAI 0.5×–0.1× | 实时 |
| Batch | **0.5× in + 0.5× out** | 分钟–约 24h |
| Flex | token 约 50% 折扣，仍是实时 API | 比默认慢 |

**Read (读):**
- [docs — OpenAI API 参考：Chat Completions / Responses——一种 E5 方言。](https://platform.openai.com/docs/api-reference)
- [docs — Anthropic Messages API：第二种 E5 方言；`cache_control` 住在这里。](https://platform.claude.com/docs/en/api/messages)
- [docs — AWS Bedrock Converse：第三种契约（默认不是 OpenAI 兼容）。](https://docs.aws.amazon.com/bedrock/latest/userguide/conversation-inference.html)
- [repo — vLLM：没买 E2 时你自己跑的 E1 引擎。](https://github.com/vllm-project/vllm)
- [repo — SGLang：带自己 runtime 和结构化生成的 E1 引擎。](https://github.com/sgl-project/sglang)
- [docs — 硅基流动：托管 E2+E5 SKU。](https://www.siliconflow.com)
- [docs — Fireworks AI：坐在 E2 的托管推理。](https://fireworks.ai)
- [docs — Together AI：托管推理 + FT（E2+E3）。](https://www.together.ai)
- [docs — Anthropic prompt caching：官方 cache-read input **0.1×**（E4）。](https://platform.claude.com/docs/en/build-with-claude/prompt-caching)

[卷一](./README.md)
