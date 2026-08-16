# Ch 24 · Permission / Identity

Identity ≠ permission. OIDC = who. OAuth = what this ticket can do. Sandbox = even if the ticket is wrong, it cannot leave the box. Audit = afterwards.

## Concept

**Definition.** Human identity answers which user ordered this. Agent identity answers which agent instance. Delegation is the user letting the agent act (OAuth 2.1 + PKCE; short audience-bound tickets, often <1h). Tool RBAC is “may it `delete_repo`?” — not in the MCP spec; runtime re-check, deny-by-default. Sandbox is an OS boundary, not authorization.

**Why it exists.** Local STDIO MCP ≈ the user’s OS permissions. HTTP MCP is the central auth point. Pattern: token vault holds the user’s GitHub/Slack ticket; runtime mints an **audience-bound token（受众绑定令牌）**; secrets stay out of the model context.

**Mechanism.** MCP HTTP (2025-06): server is an OAuth **Resource Server**; clients **MUST** send RFC 8707 `resource` on authorize and token requests (anti-replay / confused deputy). Per-tool audience; never pass the user’s long ticket down. Audit: `agent_id` + `user_sub` + tool + args hash + time.

**Boundaries.** Putting ACL in the system prompt is not F4. Skills that run `scripts/` inherit the sandbox, not extra OAuth. Installing an MCP server = granting that server’s scopes ([Ch 31](../part-06-ingress-market/ch-31-marketplace.md)).

**Failure modes.** Confused deputy: valid ticket lured to another service. Secrets in the window (they get compacted, logged, leaked). Long-lived user tokens forwarded to tools.

## Comparison

| Layer | Question | 2025–2026 mechanism |
| --- | --- | --- |
| Human identity | which user ordered this | OIDC / IdP (Entra, Okta) |
| Agent identity | which agent instance | NHI; per-agent `client_id` |
| Delegation | user lets agent act | OAuth 2.1 + PKCE; short audience-bound tickets (often <1h) |
| MCP HTTP | how remote tools auth | Resource Server + RFC 8707 resource indicator (anti-replay) |
| Tool RBAC | may it `delete_repo`? | not in the spec; runtime re-check, deny-by-default |
| Confused deputy | valid ticket lured to another service | per-tool audience; never pass the user’s long ticket down |
| Sandbox | OS boundary for code | container / VM / seccomp — not authorization |
| Audit | who, as whom, called what | `agent_id` + `user_sub` + tool + args hash + time |

**Read (读):**
- [docs — MCP Authorization (2025-06-18): Resource Server; RFC 8707 `resource` required.](https://modelcontextprotocol.io/specification/2025-06-18/basic/authorization)
- [docs — RFC 8707 Resource Indicators: `resource` binds the token to one audience.](https://www.rfc-editor.org/rfc/rfc8707.html)
- [docs — MCP architecture: where HTTP MCP sits vs STDIO.](https://modelcontextprotocol.io/docs/learn/architecture)
- [docs — OAuth 2.1 draft: PKCE and the current auth shape agents copy.](https://datatracker.ietf.org/doc/html/draft-ietf-oauth-v2-1-13)
- [docs — RFC 8707 is the anti-confused-deputy piece; pair with MCP’s MUST.](https://www.rfc-editor.org/rfc/rfc8707.html)
- [docs — Anthropic MCP connector docs (current): how Claude products attach remote MCP with OAuth.](https://platform.claude.com/docs/en/agents-and-tools/mcp)

[Part V](./README.md)
