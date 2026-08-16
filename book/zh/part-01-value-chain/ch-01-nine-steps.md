# 第 1 章 · 9 步草图

口头地图：数据 → 训练 → LLM → 部署 → Inference → API → Gateway → 应用 → 用户。

## 概念

**定义。** 这是 LLM **行业**的 30 秒对齐图，不是编程教程。每一格*可以*是一家公司、一个计费单位、一种失败模式。用来迅速说清「硅基流动在 5，OpenRouter 在 7，Cursor 在 8」。不要当投资图：有的格子是整条供应链；第 6 步（HTTP 契约）很少是独立公司。

**为什么存在。** 新闻和产品发布一次只亮一个 SKU（库存单元）。初学者会把「做模型的那家」当成整个行业。草图的用处：给新名字找一个格子，而不是把每次发布当成新宇宙。

**机制。** 按 1→9 走「谁收钱、什么会坏」。数据（标注 / 偏好 / eval）按小时或按条买。训练烧 GPU-hour。「得到 LLM」要么是闭源 API 卡，要么是开源权重文件。部署是引擎 + 集群（vLLM / SGLang），不是托管产品。Inference 是托管 token 流。API 是这条流的契约。Gateway 管密钥、fallback、多模型比价。应用是用户产品。使用者 ≠ 买方。

**边界。** 第 7 步是 **三件事**，不是一个路由器产品：[上下文层](../part-03-context-router/ch-11-definition.md) = 这段历史进不进窗口；[Model Router](../part-04-routing-nlu/ch-18-model-router.md) = 打哪颗模型；[Permission](../part-05-agent-runtime/ch-24-permission-identity.md) = 能调哪些工具、以谁的身份。不要揉成一团。第 4 步（引擎）≠ 第 5 步（托管推理）。Cursor 可跳过第 7 步直连 Anthropic/OpenAI；中转、企业统一出口、多模型比价才把 Gateway 变成刚需。

**例子。** 硅基流动把引擎 + 托管推理 + OpenAI 兼容 API 做成 **一个 SKU**。AWS Bedrock 是多模型托管 + API + VPC/IAM——一份云合约，不是「只卖 token」。Helicone / Portkey 卖计量和 traces；路由只是门，不是产品本身。

**失败模式。** 把 Scale AI 当成「全部数据」（他们是标注 / 偏好 / eval，不是 Common Crawl）。把「训练」当成一条成本曲线（预训练和后训练不同——[第 4 章](./ch-04-train-and-posttrain.md)）。把 cache miss 诊断成「Gateway 蠢」，其实是应用改写了前缀（[第 8 章](../part-02-inference-cost/ch-08-prompt-cache.md)）。

## 对比

| 步 | 名称 | 他们的例 | 实际坐落 | 计费 | 诊断 |
| --- | --- | --- | --- | --- | --- |
| 1 | 数据 | Scale AI | 标注 / 偏好 / eval，不是全部数据 | 小时 / 条 / token | 过窄 |
| 2 | 训练 | OpenAI / DeepSeek / 阿里 | 预训练 **和** 后训练压在一起 | GPU-hour | 合并过重 |
| 3 | 得到 LLM | GPT / Claude / Qwen / Llama | 闭源 API **或** 开源权重 | 模型卡 / 许可证 / API | 可用 |
| 4 | 部署 | vLLM / SGLang / TensorRT-LLM | 引擎 + 集群，不是托管产品 | GPU 租用 | 与 5 易混 |
| 5 | Inference | 硅基流动 / Fireworks / Groq | 托管 token 流 | 输入/输出 token | 真市场 |
| 6 | API | OpenAI-compatible HTTP | 第 5 步的契约 | RPM / TPM / $ | 常非独立层 |
| 7 | Gateway / Router | OpenRouter / LiteLLM / 中转站 | 密钥、fallback、多模型 | 决策 + 加成 | 可选但真实 |
| 8 | 应用 | Cursor | 用户产品，不只 IDE | 座位 / 订阅 / 加成 | 过粗 |
| 9 | 用户 | 终端用户 | 买方 ≠ 使用者 | 时间 / 结果 / 座位 | 可用 |

**一个 SKU，三顶帽子：**

| 公司 | 同时覆盖 | 买方实际买的 |
| --- | --- | --- |
| 硅基流动 SiliconFlow | 引擎 + 托管推理 + OpenAI 兼容 API（+ 微调） | token，可选 FT |
| AWS Bedrock | 多模型托管 + API + VPC/IAM | 云合约 |
| Helicone / Portkey | Gateway + traces + 密钥 + 账单 | 计量/追踪；路由只是门 |

缺口在 [第 2 章](./ch-02-refined-26.md)。

**Read (读):**
- [docs — Scale AI 官网：9 步草图停在第 1 步的标注 / 偏好 / eval 公司，不是「全部数据」。](https://scale.com)
- [repo — vLLM：开源高吞吐 serving 引擎；这是第 4 步（部署），不是托管 SKU。](https://github.com/vllm-project/vllm)
- [repo — SGLang：带自己调度和结构化生成的 serving 运行时；与 vLLM 并列的第 4 步引擎。](https://github.com/sgl-project/sglang)
- [docs — 硅基流动 SiliconFlow：托管推理 + OpenAI 兼容 API 做成一个 SKU（第 4+5+6 步）。](https://www.siliconflow.com)
- [docs — Fireworks AI：坐在第 5 步的托管推理市场。](https://fireworks.ai)
- [docs — Together AI：托管推理加微调任务；一家厂商同时做 E2 和 E3。](https://www.together.ai)
- [docs — OpenRouter：多模型 HTTP 网关（第 7 步），统一账单。](https://openrouter.ai/docs)
- [repo — LiteLLM：自托管代理，翻译厂商协议并做 retry/fallback；是 Gateway 软件，不是模型。](https://github.com/BerriAI/litellm)

[卷一](./README.md) · [本版目录](../README.md)
