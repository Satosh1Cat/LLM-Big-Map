# 第 12 章 · L0–L4 窗口

从稳定到易变的五层，好让 [prompt cache](../part-02-inference-cost/ch-08-prompt-cache.md) 有前缀可打，compacting 只打对的那一层。

## 概念

**定义。** 这是打包纪律，不是厂商。L0 协议前缀（tools schema、system、skill 的 **name+description** 而已）必须字节稳定。L1 钉住（用户规则、仓库地图、身份摘要、handoff brief）不变就还能命中。L2 记忆（compaction 块 / gist / Mamba→文本）每次 compact 后 miss。L3 活尾巴（最近 N 轮）每轮变长。L4 当前话永远不缓存。

**为什么存在。** Cache API *奖励* L0 字节稳定的客户端（Anthropic **0.1×** read）。Compacting 若改写 L0，就是在跟账单作对。把整份 Skill 正文或爆炸的 tool schema 放进 L0，前缀必爆。

**机制。** CONTINUE：只追加 L3/L4。COMPACT：改写 L2，不动 L0。RESET：清 L2–L3，留 L0（+ 部分 L1）。换模型：用新厂商方言重建 L0；旧 cache 和加密 blob 作废。OpenAI 隐式 breakpoint 打在最新 user/tool——前缀里的时间戳会 miss。Anthropic 允许 4 个显式切点；自动模式会把切点往尾巴挪（不等于冻住的 L0 切口）。

**边界。** L0 稳定不是「把 prompt 写得更好」。它是成本约束。「每轮按意图重写 system」是意图层的习惯，会毁掉 L0。OpenAI 加密 compact item 住在 L2，**不能**换厂商。

**例子。** Cursor 方向：少塞、懒取、tool 写文件不内联——前缀保持热。Codex 在窗口 **90%** 时经 Responses compact 改写 L2，再最多重读 5 个最近文件（文件系统当 L2 溢出）。

**失败模式。** Skill 正文进 L0。tool 结果日志炸弹从不清理（Anthropic 为此提供 `clear_tool_uses`）。`truncation: auto` 丢掉 L3 并拆掉哈希。

## 对比

| 层 | 内容 | Cache | 谁写 |
| --- | --- | --- | --- |
| L0 协议前缀 | tools schema、system、skill **name+description** 而已 | 必须字节稳定 | 产品 / MCP |
| L1 钉住 | 用户规则、仓库地图、身份摘要、handoff brief | 不变则命中 | 用户 + ingest |
| L2 记忆 | compaction 块 / gist / Mamba→文本 | 每次 compact 后 miss | compact 管线 |
| L3 活尾巴 | 最近 N 轮 user/assistant/tool | 每轮变长 | session |
| L4 当前话 | 当前话语 | 永不缓存 | 用户 |

**Read (读):**
- [docs — OpenAI prompt caching：隐式切点在最新 user/tool；前缀里的时间戳会 miss。](https://developers.openai.com/api/docs/guides/prompt-caching)
- [docs — Anthropic prompt caching：4 个显式 breakpoint；自动模式把切口往尾巴挪。](https://platform.claude.com/docs/en/build-with-claude/prompt-caching)
- [docs — Anthropic compaction：占据 L2、必须带回的可读块。](https://platform.claude.com/docs/en/build-with-claude/compaction)
- [docs — OpenAI compaction：不透明 L2 item；返回窗口原样再用。](https://developers.openai.com/api/docs/guides/compaction)
- [docs — Anthropic 有效上下文工程：最小高信号集；工具/skills 渐进披露。](https://www.anthropic.com/engineering/effective-context-engineering-for-ai-agents)
- [docs — Claude cookbook：清 tool vs compact vs memory——各打哪一层。](https://platform.claude.com/cookbook/tool-use-context-engineering-context-engineering-tools)
- [docs — Agent Skills 规范：`SKILL.md` 元数据常驻、正文按需——这一刀就是 L0 vs 更后层。](https://agentskills.io/specification)

[卷三](./README.md)
