# 第 10 章 · 中转站与折扣

中转站的毛利是**上游价差**，不是自训模型。形态是「你的应用吃你的企业价」，不是「把一把 Anthropic 企业钥匙拆成零售钥匙」。

## 概念

**定义。** 中转是转卖*别人的*推理入口。合法通道：合约档位、官方 cache/Batch/Flex 乘数、创业额度。价差 = 中转付给上游的 − 它向你收的。**不是**新模型。

**为什么存在。** 应用想一把钥匙、一张发票，也许还要中国可达。实验室卖量承诺。缝是市场——也是合规陷阱。

**机制。** 企业 API：年承诺 / 量，相对标价常 **5–40%**（看你的合同，不是博客）。Prompt cache：重复前缀 0.1×–0.5× input → 推理节省，前缀黏住才可能变成价差。Batch 官方 **50%**。Flex 实时约 50%。云 / ISV / 教育额度：过期、绑产品、禁转售。若中转把每个请求喷到不同上游或改写前缀，cache 价差消失。黏性 = 和 L0 同一套纪律。

**边界。** 本章**不**列破解钥匙市场。企业合同通常**禁止**把钥匙转售给未签约的终端用户。中转站是 F3+F1，不是 C1。OpenRouter 是有文档的多模型网关、有自己的条款——和法律上的「非官方中转」不是同一个东西。

**例子。** 公司签了 Anthropic 承诺，经自己的 LiteLLM、L0 冻住，可以把企业折扣和 0.1× cache *留给自己*。把那把钥匙拆给匿名零售用户是违约，不是 SKU。

**失败模式。** 广告「0.1×」却每请求换上游。把创业额度当转售库存。HTTP 长得像 OpenAI-compat 就忽略 KYC 条款。

## 对比 — 价差从哪来

| 来源 | 机制 | 谁付钱 | 约束 |
| --- | --- | --- | --- |
| 企业 API | 年承诺 / 量，相对标价常 **5–40%** | 实验室销售 | 最低消费、开票主体、驻留 |
| Prompt cache | 重复前缀 0.1×–0.5× input | 推理节省 → 价差 | 字节稳定前缀；换模型 = miss |
| 云额度 | 创业 / 市场券 | 云 / 实验室市场 | 过期、绑产品、禁转售 |
| ISV / 教育补贴 | 官方 token 赠与 | 增长预算 | KYC、审计 |
| Batch | 官方 **50%** | 填谷 | 约 24h；不是 agent 循环 |
| Flex | 约 50%，实时 API | 低峰 | 更高延迟 |

中国渠道表：[第 7 章](../part-01-value-chain/ch-07-control-plane.md)。

**Read (读):**
- [docs — Anthropic prompt caching：官方 **0.1×** read，中转只有前缀黏住才能保住。](https://platform.claude.com/docs/en/build-with-claude/prompt-caching)
- [docs — OpenAI Batch：官方 **50%**——价目表 SKU，不是私下成交。](https://platform.openai.com/docs/guides/batch)
- [docs — OpenAI Flex：官方实时约 50% 折扣。](https://platform.openai.com/docs/guides/flex-processing)
- [docs — Anthropic 商业条款（当前公开条款）：转售和可接受使用写在合同里，不在博客里。](https://www.anthropic.com/legal/commercial-terms)
- [docs — OpenAI 使用政策：API 客户不能拿钥匙做什么。](https://openai.com/policies/usage-policies)
- [repo — LiteLLM：很多企业用来代替非官方中转的自托管 F3。](https://github.com/BerriAI/litellm)
- [docs — OpenRouter 文档：有文档的多模型网关、自己的账单，不是克隆的企业钥匙。](https://openrouter.ai/docs)

[卷二](./README.md)
