# 第 8 章 · Prompt cache（提示缓存）

不是语义缓存（「相似问题复用答案」）。是**精确前缀** KV 复用。

## 概念

**定义。** Prompt cache 是 transformer KV 的**逐字节前缀**复用。稳定前缀（tools + system + 历史前半）被哈希。命中 → 更便宜/更快的 input。breakpoint 前改一个字节 → 从该点 miss。换模型 → miss。**不是** embedding 相似，也不是「缓存答案」。

**为什么存在。** Agent 循环每一轮都重发同一份 tool schema 和 system。没有前缀复用，窗口变长你就付全价 input。所以 [第 12 章](../part-03-context-router/ch-12-l0-l4-window.md) 的 L0「稳定」是*账单*，不是口味。

**机制 — Anthropic（显式）。** 在内容块上标 `cache_control: { type: "ephemeral", ttl?: "5m" | "1h" }`。每请求最多 **4** 个 breakpoint。Write：5m **1.25×**，1h **2.0×**。Read：基价 input 的 **0.1×**（官方）。2026-02 起 Claude API 为 workspace 隔离（不再 org 共享）。

**机制 — OpenAI（多为隐式）。** 合格模型自动缓存最长精确前缀；GPT-5.6+ 可再打显式 breakpoint。最短约 1024 tokens。Write：GPT-5.6+ 未缓存部分 **1.25×**；更早常无 write 费。Read：约 0.5×（gpt-4o）→ 0.25×（gpt-4.1）→ **0.1×**（gpt-5 系）。TTL：GPT-5.6+ 30 分钟精确；更早 best-effort 5–60 分钟。`prompt_cache_key` 把相似流量分片，避免互相挤掉。

**边界。** Compacting 在前缀里插入新块 → 从该点 miss（[第 14 章](../part-03-context-router/ch-14-compacting.md)）。每轮按意图重写 system 是在跟 0.1× 作对。对*查询*做 embedding 的语义路由打不中这个缓存。LiteLLM 必须原样保留 breakpoint 字节，否则命中死在网关。

**打失例子。** 稳定指令和用户话之间插时间戳 → 隐式前缀含时间戳 → 下一请求 miss。修法：在稳定块末尾打显式 breakpoint。

**失败模式。** L0 里 tool schema 乱漂。把整份 Skill 正文塞进 system。`truncation: auto` 自己丢消息、拆前缀。中转站把同一 session 喷到不同上游。

## 对比 — Anthropic 显式 vs OpenAI 隐式

| | Anthropic | OpenAI |
| --- | --- | --- |
| 标记 | `cache_control: { type: "ephemeral", ttl?: "5m" \| "1h" }` | 合格模型自动；GPT-5.6+ 可显式 breakpoint |
| 每请求切点 | 最多 **4** | 隐式打在最新 user/tool；可再显式 |
| 最短前缀 | 视模型（常 1,024+） | 约 1024（老模型 1024–2048） |
| Write | 5m **1.25×**；1h **2.0×** | GPT-5.6+：**1.25×**；更早常无 write 费 |
| Read | **0.1×** 基价 input | 约 0.5×（gpt-4o）→ 0.25×（gpt-4.1）→ **0.1×**（gpt-5 系） |
| TTL | 默认 5m，可选 1h | GPT-5.6+：30min 精确；更早 best-effort 5–60min |
| 亲和 | 2026-02 起 Claude API 为 workspace 级，不再 org 共享 | `prompt_cache_key` 分片 |
| 匹配 | 字面前缀哈希 | 精确前缀；**不是** embedding 相似 |

**Read (读):**
- [docs — Anthropic Prompt caching：官方 **0.1×** read，write 1.25×/2.0×，最多 4 个 breakpoint；不是语义缓存。](https://platform.claude.com/docs/en/build-with-claude/prompt-caching)
- [docs — OpenAI Prompt caching（API）：只对精确前缀；静态在前；`prompt_cache_key`；GPT-5.6+ write 1.25×。](https://developers.openai.com/api/docs/guides/prompt-caching)
- [docs — OpenAI Prompt Caching 201 cookbook：cache 命中时 Flex vs Batch 怎么比；带请求形态。](https://developers.openai.com/cookbook/examples/prompt_caching_201)
- [docs — Anthropic 有效上下文工程：稳定前缀是注意力预算手段，不只是折扣。](https://www.anthropic.com/engineering/effective-context-engineering-for-ai-agents)
- [docs — Claude cookbook：memory / compaction / 清 tool 是三件杠杆，都会动你想缓存的前缀。](https://platform.claude.com/cookbook/tool-use-context-engineering-context-engineering-tools)
- [docs — OpenAI compaction：压缩会改写历史，因而改写可缓存前缀。](https://developers.openai.com/api/docs/guides/compaction)
- [paper — Lost in the Middle（Liu et al.）：长窗败在*中间*；cache 修不了位置偏差。](https://arxiv.org/abs/2307.03172)

[卷二](./README.md)
