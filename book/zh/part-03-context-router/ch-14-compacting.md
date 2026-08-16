# 第 14 章 · Compacting（压缩）

对**整段** transcript（user / assistant / tool）做有损操作。目标：把工作集留在模型还能集中注意力的区域。长窗会撞 lost-in-the-middle（Liu et al. 2023）。

## 概念

**定义。** Compacting 是*对当前窗口的有损压缩*，不是无限上下文，不是 RAG，不是相关判定。T0/T1 可以无 LLM（丢掉「谢谢」，留下决定）。T2 是厂商或 LLM 摘要。T3 是私有 SSM 状态（[第 17 章](./ch-17-mamba-rnn.md)）。经典 token 压缩器（Compressive Transformer、AutoCompressor、ICAE、Gist tokens、LLMLingua）仍吐出你能拷走的 **token**；厂商加密 blob 不能。

**为什么存在。** 注意力有限。位置偏差伤害长提示的中间。Agent transcript 会随 tool 倾倒变长。你要么 compact，要么懒取，要么又付钱又变差。

**机制 — Anthropic `compact_20260112`。** 可读 compaction 块。按 `input_tokens` 触发，默认 **150k**，最低 **50k**。客户端**必须把该块带回去**；API 丢掉它之前的消息。`pause_after_compaction`、自定义 `instructions`。旁路杠杆：`clear_tool_uses_*`、`memory_*`。

**机制 — OpenAI。** 服务端 `context_management.compact_threshold` 或独立 `POST /responses/compact`。不透明加密 item（对 ZDR 友好）。返回窗口**原样**再用，不要再剪。compact 的 `instructions` 应与 Responses 的一致。`truncation: auto` 自己丢消息并**打破 cache**——那不是 compacting。

**Harness 触发。** Codex CLI：窗口 **90%**（可下调不可上调）→ OpenAI 路径加密 blob，否则本地 handoff 摘要 + 约 20k user tokens，再最多重读 5 个最近文件。Gemini CLI：默认 1M 窗的 **50%** ≈ 524k；前 70% → XML `state_snapshot`，后 30% 原文；两次 LLM。opencode：约 96%（留输出余量）。Claude Code：token/事件或模型自请 `/compact` → 可读 markdown。

**边界。** 加密 blob **不能**带到另一家——先投影成 T2 *文本*。Compacting ≠ RESET（RESET 丢掉对话正文是因为*任务*变了）。Compacting ≠ LLMLingua（可搬运的提示压缩，不是 session API）。

**失败模式。** Compact 了 L0。剪了 OpenAI 返回的窗口。把 Gemini 的 50% 触发当成「1M 可靠」。把 compact 块插在 cache breakpoint *前面*。

## 对比 — 厂商 API + harness

| | Anthropic `compact_20260112` | OpenAI `POST /responses/compact` |
| --- | --- | --- |
| 形态 | 可读 compaction 块 | 不透明加密 item（对 ZDR 友好） |
| 触发 | `input_tokens`，默认 **150k**，最低 **50k** | 客户端阈值或 `context_management.compact_threshold` |
| 之后 | 必须带回该块；API 丢掉块之前的消息 | 返回窗口**原样**再用，不要再剪 |
| 暂停 | `pause_after_compaction: true` | 独立端点本身就是显式一步 |
| 自定义 | `instructions` 整段替换默认摘要提示 | compact 的 `instructions` 应与 Responses 的一致 |
| 另外 | `clear_tool_uses_*`、`memory_*` | `truncation: auto`（自己丢消息、破 cache） |

| Harness | 触发 | 留下什么 |
| --- | --- | --- |
| Codex CLI | 窗口 **90%**，可下调不可上调 | OpenAI 路径：加密 blob；否则本地 handoff 摘要 + 约 20k user tokens；然后最多重读 5 个最近文件 |
| Gemini CLI | 默认 **50%**（1M 窗 ≈ 524k） | 前 70% → XML `state_snapshot`，后 30% 原文；两次 LLM |
| opencode | ~96%（留输出余量） | marker 之后全丢；另有非 LLM 的 tool prune（40k 保护区） |
| Claude Code | token / 事件或模型自请 `/compact` | 可读 markdown 摘要 |
| Cursor 方向 | 少塞、懒取、tool 写文件不内联 | 前缀尽量不被 tool 输出顶掉，cache 保持热 |

**Read (读):**
- [docs — Anthropic Compaction：beta `compact-2026-01-12`；默认 150k / 最低 50k；要把 `compaction` 块带回去。](https://platform.claude.com/docs/en/build-with-claude/compaction)
- [docs — OpenAI Compaction 指南：服务端 `compact_threshold` 或独立 compact；不透明 item；不要剪。](https://developers.openai.com/api/docs/guides/compaction)
- [docs — OpenAI `POST /responses/compact` API 参考：Codex 90% 路径实际调用的方法。](https://developers.openai.com/api/reference/resources/responses/methods/compact)
- [repo — openai/codex：公开的 Codex CLI harness（90% 窗口 compact 策略在这个产品里）。](https://github.com/openai/codex)
- [repo — google-gemini/gemini-cli：公开 Gemini CLI；默认约在 1M 窗的 50% compact。](https://github.com/google-gemini/gemini-cli)
- [repo — anthropics/claude-code：Claude Code harness；`/compact` → 可读 markdown。](https://github.com/anthropics/claude-code)
- [repo — anomalyco/opencode：开源 harness，高水位 compact + tool prune。](https://github.com/anomalyco/opencode)
- [paper — Lost in the Middle（Liu et al. 2023）：U 形检索；compacting 存在的原因。](https://arxiv.org/abs/2307.03172)
- [paper — LLMLingua（Jiang et al.）：由粗到细的提示压缩，可达 20×，仍是可搬运 token。](https://arxiv.org/abs/2310.05736)
- [repo — microsoft/LLMLingua：LLMLingua / LongLLMLingua / LLMLingua-2 实现。](https://github.com/microsoft/LLMLingua)
- [paper — LongLLMLingua：query-aware 长上下文压缩；ACL 2024。](https://arxiv.org/abs/2310.06839)
- [paper — Gist tokens（Mu et al.）：把提示压成可缓存的 gist 激活；不是厂商 blob。](https://arxiv.org/abs/2304.08467)
- [paper — AutoCompressor（Chevalier et al.）：递归把上下文压成摘要向量，仍是 token/soft prompt。](https://arxiv.org/abs/2305.14788)
- [paper — ICAE（Ge et al.）：in-context 自编码器，把上下文压进 memory slot。](https://arxiv.org/abs/2307.06945)
- [paper — Compressive Transformer（Rae et al.）：压缩旧记忆；仍是 token。](https://arxiv.org/abs/1911.05507)

[卷三](./README.md)
