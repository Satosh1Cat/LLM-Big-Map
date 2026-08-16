# 第 7 章 · 控制面

身份、账单、traces、Gateway、企业封装。9 步草图只画了「Gateway」。

## 概念

**定义。** 控制面是*不是* token 流、但仍决定这次调用发不发出去的一切：谁是主体（F1）、记了什么（F2）、哪把钥匙/哪颗模型/哪个区域（F3）、哪层 VPC/合规信封（F4）。Gateway 能留住客户，常常是因为密钥 + 发票 + traces，不是因为路由算法。

**为什么存在。** Cursor 可以跳过 F3 直连 Anthropic/OpenAI。多模型比价、中国中转、企业「一个出口」才让 F3 变成刚需。没有 F2，你分不清 cache miss、换模型、还是 tool schema 漂了。

**机制。** F1：组织账单、SSO、年度承诺。F2：span（LangSmith、Helicone、Langfuse）——提示、token、cache hit、延迟。F3：OpenRouter / LiteLLM / Portkey / one-api——翻译方言、池化密钥、fallback。F4：VPC、等保、DLP、Azure OpenAI 合约。中国不是 OpenRouter 的译文：中转站、云模型广场（百炼、方舟）、实验室直连 API（DeepSeek、智谱）。昇腾 ≠ CUDA。

**边界。** Gateway ≠ Model Router（[第 18 章](../part-04-routing-nlu/ch-18-model-router.md) 是*哪颗模型*；F3 是*这次调用怎么发出去*）。可观测性 ≠ 评测门。MCP 是*工具协议*（G3），不是 Gateway 产品。中转经济学：[第 10 章](../part-02-inference-cost/ch-10-relay-discount.md)。

**例子。** Helicone 挡在一把 OpenAI 钥匙前面，是 F2+F3，路由只是门。把每个请求打到不同上游的中转站会杀掉 prompt-cache 价差——黏性要求和 L0 同一套纪律。

**失败模式。** 把 ACL 写进 system prompt 就叫 F4。用了 OpenRouter `auto` 就以为有了任务感知的上下文编译器。把密钥打进 F2 traces。

## 对比

| 层 | 卖方 | 账单 |
| --- | --- | --- |
| F1 身份 / 账单 | OpenAI org, Azure EA, WorkOS SSO | 座位、承诺、超量 |
| F2 可观测性 | LangSmith, Helicone, Langfuse | span / 月 |
| F3 Gateway | OpenRouter, LiteLLM, Portkey, one-api | 加成或自托管软件 |
| F4 企业 | VPC、等保、DLP、Azure OpenAI 合约 | 云差价 + 审计 |

| 中国角色 | 例子 | 层 |
| --- | --- | --- |
| 实验室直连 API | DeepSeek, 智谱, Moonshot | C5+E2+E5 |
| 云广场 | 百炼, 方舟 | E2+F4 |
| 独立托管 | 硅基流动 | E2 |
| 中转 | one-api, new-api | F3+F1 |
| 权重分发 | ModelScope, HF 镜像 | C4+C5 |

**Read (读):**
- [docs — MCP 架构：工具作为协议（G3），不是 Gateway SKU。](https://modelcontextprotocol.io/docs/learn/architecture)
- [repo — LiteLLM：自托管 F3 代理——retry、fallback、预算、方言翻译。](https://github.com/BerriAI/litellm)
- [docs — OpenRouter：托管多模型 F3，统一账单。](https://openrouter.ai/docs)
- [repo — Portkey gateway：开源 AI 网关（密钥、fallback、guardrails）。](https://github.com/Portkey-AI/gateway)
- [repo — Helicone：可观测性优先的代理；F2，F3 只是门。](https://github.com/Helicone/helicone)
- [repo — Langfuse：开源 LLM tracing（F2）。](https://github.com/langfuse/langfuse)
- [docs — WorkOS SSO：应用侧 F1 身份管道，不是模型路由器。](https://workos.com/sso)
- [docs — 阿里云百炼：中国云广场（E2+F4），不是中文版 OpenRouter。](https://www.aliyun.com/product/bailian)

[卷一](./README.md)
