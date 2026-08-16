# 第 31 章 · Marketplace

把 Context / Skills / 浏览器能力列为可安装单元。货架已经不一样。

## 概念

**定义。** 市场列出可安装能力：GPT Store（提示 + 工具当一次聊天）、MCP 目录（工具服务器）、Skills 货架（`SKILL.md`）、整 agent 商店（harness + 身份 + 账单）。一份上架若没有**权限清单 + eval 报告 + 上下文预算**，只是目录，不是治理。

**为什么存在。** 安装 MCP 服务器 = 授予该服务器的 scopes。安装 skill = 允许额外文件、也许还有 `scripts/` 进窗口。用户把「添加」当应用商店点；运行时必须当授权。

**机制。** GPT Store：聊天里对第三方 OAuth。MCP 目录（Smithery、PulseMCP、官方 registry）：安装 = 授权那台服务器。Cursor / Claude / Codex Skills：可移植 `SKILL.md`（agentskills.io）。Agent 市场把身份 + 工具 + 账单捆在一起。

**边界。** Skills 不是 MCP。MCP 不是模型路由器。目录星数不是 eval。上下文预算必须写在上架信息里，因为一份往 L0 倒 20k token 的 skill 就是一次 cache 事故。

**失败模式。** 一键安装却没有受众绑定令牌。Skills 把自己 alwaysApply 进 L0。市场跳过 eval 报告。

## 对比

| 货架 | 卖什么 | 权限含义 |
| --- | --- | --- |
| GPT Store | 提示 + 工具当一次聊天 | 对第三方 OAuth |
| MCP 目录（Smithery, PulseMCP, 官方） | 工具服务器 | 安装 = 授权该服务器的 scope |
| Cursor / Claude / Codex Skills | `SKILL.md` 程序知识 | 读文件 + 也许跑 `scripts/` |
| Agent 市场 | 整 agent + harness | 身份 + 工具 + 账单捆在一起 |

**Read (读):**
- [docs — agentskills.io：Skills 货架真正卖的可移植单元。](https://agentskills.io/home)
- [docs — Agent Skills 规范。](https://agentskills.io/specification)
- [docs — MCP authorization：「安装这台服务器」实际授予什么。](https://modelcontextprotocol.io/specification/2025-06-18/basic/authorization)
- [docs — 官方 MCP registry / 规范入口（当前）。](https://modelcontextprotocol.io)
- [docs — Cursor skills：一个 IDE 货架如何加载 `SKILL.md`。](https://cursor.com/docs/context/skills)
- [docs — OpenAI GPT Store 帮助：聊天形态上架 + 第三方 OAuth。](https://help.openai.com/en/articles/8798878-what-is-the-gpt-store)
- [docs — Claude Agent Skills 概览。](https://platform.claude.com/docs/en/agents-and-tools/agent-skills/overview)

[卷六](./README.md)
