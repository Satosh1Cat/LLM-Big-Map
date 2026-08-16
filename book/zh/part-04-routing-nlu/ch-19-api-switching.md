# 第 19 章 · API 切换

Gateway 问题。意图**不是**「路由器」。

## 概念

**定义。** 翻译方言、池化连接、CONTINUE 时黏住模型、5xx/TPM 时故障转移。换厂商前：把窗口投影成**文本 transcript + 开放的 tool 结果**，重建 L0。

**为什么存在。** OpenAI-compat 是方言。Anthropic Messages、Gemini `contents`、Bedrock Converse 并不共享 `tool_call`、system 槽位或 `cache_control`。以为「POST JSON 就行」的应用，第一次 failover 才会发现。

**机制。** 协议层改写字段。HTTP/2 池、SSE、取消——能在首字节前 failover 就不要等到半截。黏性：`session_id` → `model_id` 直到 SHIFT / 能力缺口 / 5xx，以保护 cache + compact blob。健康：P90、TPM 余量、区域、价格。Fallback：LiteLLM / Portkey 有序链。密钥：企业池、公平 RPM——中转真正的工作。

**带不过去的：** OpenAI 加密 compact item、Anthropic cache 哈希、专有 reasoning 块、厂商特有的 tool-result 形状。

**边界。** 不是 NLU。不是任务感知 compile。不是「自动选最聪明的模型」，除非你明确加了 [第 18 章](./ch-18-model-router.md)。

**失败模式。** 半流 failover 还留着旧 compact blob。把 `cache_control` 翻译成空操作，再奇怪 0.1× 为什么没了。

## 对比

| 层 | 技术 | 工作 |
| --- | --- | --- |
| 协议 | OpenAI-compat；Anthropic Messages ↔ Chat；Gemini contents | `tool_call`、system 槽、`cache_control` 过不了 |
| 连接 | HTTP/2 池、SSE、取消 | 首字节前 failover |
| 黏性 | `session_id` → `model_id` 直到 SHIFT / 能力缺口 / 5xx | 保护 cache + compact blob |
| 健康 | P90、TPM 余量、区域、价格 | 加权随机 / 水位 |
| Fallback | LiteLLM / Portkey 有序链 | 超时 → 下一个；改写 messages |
| 密钥 | 企业钥匙池、公平 RPM | 中转真正的工作 |

**Read (读):**
- [docs — LiteLLM 负载均衡 / fallback：有序链，不是 NLU。](https://docs.litellm.ai/docs/routing)
- [repo — Portkey-AI/gateway：方言翻译 + fallback。](https://github.com/Portkey-AI/gateway)
- [docs — Anthropic Messages API：`cache_control` 真正住的方言。](https://platform.claude.com/docs/en/api/messages)
- [docs — OpenAI compaction：换厂商时会死的东西（加密 item）。](https://developers.openai.com/api/docs/guides/compaction)
- [docs — Anthropic compaction：可读块仍偏向原厂商。](https://platform.claude.com/docs/en/build-with-claude/compaction)
- [docs — OpenRouter provider routing：托管 F3 怎么挑上游。](https://openrouter.ai/docs/guides/routing/provider-selection)
- [docs — Gemini generateContent：第三种 body（`contents`），不是 Chat Completions。](https://ai.google.dev/api/generate-content)

[卷四](./README.md)
