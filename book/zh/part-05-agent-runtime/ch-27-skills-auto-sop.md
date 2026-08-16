# 第 27 章 · Skills / 自动 SOP

Skill = 按需剧本。主循环 = 上下文打包 + 工具周期。不要把每份 SOP 倒进 system（L0 膨胀、打破 cache、变成关不掉的「主功能」）。

## 概念

**定义。** Skill 是带 `SKILL.md` 的目录：YAML `name` + `description`（description 带着**何时触发**），然后是 markdown 正文，可选 `scripts/` / `references/` / `assets/`。渐进披露：元数据常驻；正文只有命中才进窗口。

**为什么存在。** 规则是短约束。主循环每轮都跑。Skills 在中间：多步程序，不相关时不该占 L0。自动 SOP 挖掘是**旁路**（聚类 traces → Batch 起草 `SKILL.md` → eval A/B → 人工审 `description`）。

**机制。** Cursor 发现 `.agents/skills/`、`.cursor/skills/`，兼容还读 `.claude/skills` / `.codex/skills`。Agent Skills 是开放标准（agentskills.io）。Claude Agent Skills 概览是同一套渐进披露的官方叙述。

**边界。** Skills ≠ MCP 服务器（MCP 授予的是*工具*和 scopes）。Skills ≠ 永远开启的规则。`disable-model-invocation` 让 skill 只能斜杠调用。

**失败模式。** 把整本 runbook 写进 `description`（废掉渐进披露）。自动挖的 skill 不经 eval。脚本逃出 sandbox。

## 对比 — Skills vs 规则 vs 主循环

| | 规则 | Skills | 主 agent 循环 |
| --- | --- | --- | --- |
| 体量 | 短约束 | 多步 + 可选 `scripts/` | 每轮 |
| 加载 | 匹配 → 进窗口 | 索引 name+description；命中 → 读 `SKILL.md` | 永远 |
| 跨产品 | `.cursorrules` 等 | [agentskills.io](https://agentskills.io/home)；Cursor 也读 `.claude/skills` 与 `.codex/skills` | 各家自有 |

**Read (读):**
- [docs — agentskills.io 首页：可移植目录格式；渐进披露。](https://agentskills.io/home)
- [docs — Agent Skills 规范：`SKILL.md` 前置元数据与布局。](https://agentskills.io/specification)
- [docs — Cursor skills：Cursor 从哪加载、`/` 调用、兼容目录。](https://cursor.com/docs/context/skills)
- [docs — Claude Agent Skills 概览：官方渐进披露。](https://platform.claude.com/docs/en/agents-and-tools/agent-skills/overview)
- [repo — anthropics/claude-code：会读 `.claude/skills` 的 harness。](https://github.com/anthropics/claude-code)
- [repo — openai/codex：会读 `.codex/skills` 的 harness。](https://github.com/openai/codex)

[卷五](./README.md)
