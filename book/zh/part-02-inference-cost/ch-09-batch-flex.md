# 第 9 章 · Batch 与 Flex

用**延迟**换**填谷**。官方价目，不是私下成交。

## 概念

**定义。** **Batch** 是离线队列：提交 JSONL，厂商填低谷，完成窗口 **约 24h**。OpenAI 和 Anthropic 相对同步 API 标价 **输入和输出都 50%**。**Flex** 仍是**实时** API（`service_tier=flex`），token 约 50% 折扣，延迟更高。两者都不是破解钥匙。

**为什么存在。** GPU 有空谷。实验室宁愿便宜卖掉，也不愿空转。Agent 主循环（人盯着光标）等不了 24 小时。离线评测、embedding 回填、夜间 SOP 蒸馏可以。

**机制 — Batch。** OpenAI `POST /batches` 或 Anthropic Message Batches。JSONL 行带唯一 `custom_id`。与实时池分开限流。Prompt cache *可以*叠，但分片/TTL 不如黏住的交互 session 稳。结果是另一份 JSONL。

**机制 — Flex。** 还是 Responses / Chat 那个 URL，加 `service_tier=flex`。因为仍是实时，流式和 `prompt_cache_key` 还在。延迟比默认差；图的就是价。

**边界。** Batch ≠ Flex。Flex ≠ prompt cache（cache 是你已选档位*之上*的前缀折扣）。Batch 适合 [evals](../part-05-agent-runtime/ch-25-agent-evals.md) 和 [自动 SOP](../part-05-agent-runtime/ch-27-skills-auto-sop.md)；人在等的 [ChatGPT ingest](../part-06-ingress-market/ch-30-chatgpt-handoff.md) 不行——除非 ingest 放过夜。

**例子。** Cookbook 轶事（不是*你的*流量）：同样 1 万次重复请求，Flex+延长 cache 比 Batch 的 cache 命中高约 8.5%、input 成本低约 23%——因为 Flex 保住实时 cache 语义。当假说，再自己量。

**失败模式。** 把 agent 主循环放上 Batch。把 Batch 50% 和 Flex 50% *再乘* cache 0.1× 当成一个乘数。忘了 `custom_id`，结果对不回去。

## 对比 — Batch 50%/24h vs Flex 50% 实时

| | 交互 | Batch | Flex |
| --- | --- | --- | --- |
| 延迟 | 秒 | 分钟–24h | 比默认实时慢 |
| 价格 | 1.0× | **0.5× in + 0.5× out** | token 约 50% 折扣 |
| Cache | 完整 prompt-cache 语义 | 能叠；分片/TTL 较差 | 实时语义 |
| 协议 | `/chat/completions`, `/messages`, `/responses` | `POST /batches` + JSONL | Responses + `service_tier=flex` |
| 用途 | agent 循环 | evals、embedding 回填、夜间 SOP | 能忍的准实时 |

**Read (读):**
- [docs — OpenAI Batch API：官方相对同步 **50%**，24h 窗口，独立限流，JSONL + `custom_id`。](https://platform.openai.com/docs/guides/batch)
- [docs — Anthropic Message Batches：Messages 的官方 50% 批价与 24h 处理。](https://platform.claude.com/docs/en/build-with-claude/batch-processing)
- [docs — OpenAI Flex processing：实时 API 上约 50%，靠 `service_tier=flex`；不是 24h 队列。](https://platform.openai.com/docs/guides/flex-processing)
- [docs — OpenAI Prompt Caching 201：账单被 cache 命中主导时，Flex vs Batch。](https://developers.openai.com/cookbook/examples/prompt_caching_201)
- [docs — OpenAI prompt caching：两种 SKU 上都能叠（或叠不上）的前缀折扣。](https://developers.openai.com/api/docs/guides/prompt-caching)
- [docs — Anthropic prompt caching：前缀稳定时 **0.1×** read 在 Batch/交互上仍适用。](https://platform.claude.com/docs/en/build-with-claude/prompt-caching)

[卷二](./README.md)
