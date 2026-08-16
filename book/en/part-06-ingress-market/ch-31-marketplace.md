# Ch 31 · Marketplace

List Context / Skills / browser capabilities as installable units. Shelves already differ.

## Concept

**Definition.** A marketplace lists installable capabilities: GPT Store (prompt + tools as a chat), MCP catalogs (tool servers), Skills shelves (`SKILL.md`), whole-agent stores (harness + identity + billing). A listing without **permission manifest + eval report + context budget** is a directory, not governance.

**Why it exists.** Installing an MCP server = granting that server’s scopes. Installing a skill = allowing extra files and maybe `scripts/` into the window. Users click “add” as if it were an app store; the runtime must treat it as authorization.

**Mechanism.** GPT Store: OAuth to third parties inside a chat. MCP catalogs (Smithery, PulseMCP, official registry): install = authorize that server. Cursor / Claude / Codex Skills: portable `SKILL.md` (agentskills.io). Agent marketplaces bundle identity + tools + billing.

**Boundaries.** Skills are not MCP. MCP is not a model router. A catalog star count is not an eval. Context budget belongs on the listing because a skill that dumps 20k tokens into L0 is a cache incident.

**Failure modes.** One-click install with no audience-bound token. Skills that alwaysApply themselves into L0. Marketplaces that skip eval reports.

## Comparison

| Shelf | Sells | Permission meaning |
| --- | --- | --- |
| GPT Store | prompt + tools as a chat | OAuth to third parties |
| MCP catalogs (Smithery, PulseMCP, official) | tool servers | install = authorize that server’s scope |
| Cursor / Claude / Codex Skills | `SKILL.md` procedural knowledge | read files + maybe run `scripts/` |
| Agent marketplaces | whole agent + harness | identity + tools + billing bundled |

**Read (读):**
- [docs — agentskills.io: the portable unit a Skills shelf actually sells.](https://agentskills.io/home)
- [docs — Agent Skills specification.](https://agentskills.io/specification)
- [docs — MCP authorization: what “install this server” grants.](https://modelcontextprotocol.io/specification/2025-06-18/basic/authorization)
- [docs — Official MCP registry / spec entry (current).](https://modelcontextprotocol.io)
- [docs — Cursor skills: how one IDE shelf loads `SKILL.md`.](https://cursor.com/docs/context/skills)
- [docs — OpenAI GPT Store help: chat-shaped listings + third-party OAuth.](https://help.openai.com/en/articles/8798878-what-is-the-gpt-store)
- [docs — Claude Agent Skills overview.](https://platform.claude.com/docs/en/agents-and-tools/agent-skills/overview)

[Part VI](./README.md)
