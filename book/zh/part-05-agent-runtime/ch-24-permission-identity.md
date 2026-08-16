# 第 24 章 · Permission / Identity

身份 ≠ 权限。OIDC = 是谁。OAuth = 这张票能做什么。Sandbox = 即便票错了也出不了盒子。Audit = 事后。

## 概念

**定义。** 人类身份回答是哪个用户下的令。Agent 身份回答是哪个 agent 实例。委托是用户让 agent 去办事（OAuth 2.1 + PKCE；短的受众绑定票，常常 <1h）。工具 RBAC 是「能不能 `delete_repo`」——不在 MCP 规范里；运行时再查，默认拒绝。Sandbox 是 OS 边界，不是授权。

**为什么存在。** 本地 STDIO MCP ≈ 用户的 OS 权限。HTTP MCP 才是中央鉴权点。模式：token vault 握着用户的 GitHub/Slack 票；运行时签发**受众绑定令牌**；秘密不进模型上下文。

**机制。** MCP HTTP（2025-06）：服务器是 OAuth **Resource Server**；客户端在授权和换票时 **MUST** 带 RFC 8707 `resource`（防重放 / confused deputy）。按工具分 audience；绝不把用户的长票往下传。审计：`agent_id` + `user_sub` + 工具 + 参数哈希 + 时间。

**边界。** 把 ACL 写进 system prompt 不是 F4。会跑 `scripts/` 的 Skills 继承的是 sandbox，不是额外 OAuth。安装一个 MCP 服务器 = 授予该服务器的 scopes（[第 31 章](../part-06-ingress-market/ch-31-marketplace.md)）。

**失败模式。** Confused deputy：有效票被诱到另一个服务。秘密进了窗口（随后被 compact、被记日志、被泄漏）。把长寿命用户票转发给工具。

## 对比

| 层 | 问题 | 2025–2026 机制 |
| --- | --- | --- |
| 人类身份 | 哪个用户下的令 | OIDC / IdP（Entra, Okta） |
| Agent 身份 | 哪个 agent 实例 | NHI；每 agent `client_id` |
| 委托 | 用户让 agent 去办 | OAuth 2.1 + PKCE；短受众绑定票（常 <1h） |
| MCP HTTP | 远程工具怎么鉴权 | Resource Server + RFC 8707 resource indicator（防重放） |
| 工具 RBAC | 能不能 `delete_repo`？ | 规范不管；运行时再查，默认拒绝 |
| Confused deputy | 有效票被诱到另一服务 | 按工具 audience；绝不下传用户长票 |
| Sandbox | 代码的 OS 边界 | 容器 / VM / seccomp — 不是授权 |
| 审计 | 谁、以谁的身份、调了什么 | `agent_id` + `user_sub` + 工具 + 参数哈希 + 时间 |

**Read (读):**
- [docs — MCP Authorization（2025-06-18）：Resource Server；必须带 RFC 8707 `resource`。](https://modelcontextprotocol.io/specification/2025-06-18/basic/authorization)
- [docs — RFC 8707 Resource Indicators：`resource` 把票绑到一个 audience。](https://www.rfc-editor.org/rfc/rfc8707.html)
- [docs — MCP 架构：HTTP MCP 相对 STDIO 坐在哪。](https://modelcontextprotocol.io/docs/learn/architecture)
- [docs — OAuth 2.1 draft：PKCE 以及 agent 在抄的当前鉴权形状。](https://datatracker.ietf.org/doc/html/draft-ietf-oauth-v2-1-13)
- [docs — RFC 8707 是反 confused-deputy 的那一块；和 MCP 的 MUST 配对。](https://www.rfc-editor.org/rfc/rfc8707.html)
- [docs — Anthropic MCP connector（当前）：Claude 产品如何用 OAuth 接远程 MCP。](https://platform.claude.com/docs/en/agents-and-tools/mcp)

[卷五](./README.md)
