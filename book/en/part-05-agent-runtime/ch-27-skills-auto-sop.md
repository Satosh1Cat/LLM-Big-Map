# Ch 27 · Skills / auto-SOP

Skill = on-demand playbook. Main loop = context packing + tool cycle. Do not dump every SOP into system (that bloats L0, breaks cache, becomes an un-disableable “main feature”).

## Concept

**Definition.** A skill is a folder with `SKILL.md`: YAML `name` + `description` (description carries **when to trigger**), then a markdown body, optional `scripts/` / `references/` / `assets/`. Progressive disclosure: metadata always loaded; body enters the window only when hit.

**Why it exists.** Rules are short constraints. The main loop runs every turn. Skills are the middle: multi-step procedures that should not occupy L0 until relevant. Auto-SOP mining is a **side path** (cluster traces → Batch draft `SKILL.md` → eval A/B → human-review `description`).

**Mechanism.** Cursor discovers `.agents/skills/`, `.cursor/skills/`, and for compatibility `.claude/skills` / `.codex/skills`. Agent Skills is an open standard (agentskills.io). Claude Agent Skills overview documents the same progressive-disclosure idea first-party.

**Boundaries.** Skills ≠ MCP servers (MCP grants *tools* and scopes). Skills ≠ always-on rules. `disable-model-invocation` makes a skill slash-only.

**Failure modes.** Putting the whole runbook in `description` (defeats progressive disclosure). Auto-mined skills without eval. Scripts that escape the sandbox.

## Comparison — Skills vs Rules vs main loop

| | Rules | Skills | Main agent loop |
| --- | --- | --- | --- |
| Volume | short constraints | multi-step + optional `scripts/` | every turn |
| Load | match → into window | index name+description; hit → read `SKILL.md` | always |
| Cross-product | `.cursorrules` etc. | [agentskills.io](https://agentskills.io/home); Cursor also reads `.claude/skills` and `.codex/skills` | each vendor’s own |

**Read (读):**
- [docs — agentskills.io home: portable folder format; progressive disclosure.](https://agentskills.io/home)
- [docs — Agent Skills specification: `SKILL.md` frontmatter and layout.](https://agentskills.io/specification)
- [docs — Cursor skills: where Cursor loads skills, `/` invoke, compatibility dirs.](https://cursor.com/docs/context/skills)
- [docs — Claude Agent Skills overview: first-party progressive disclosure.](https://platform.claude.com/docs/en/agents-and-tools/agent-skills/overview)
- [repo — anthropics/claude-code: a harness that reads `.claude/skills`.](https://github.com/anthropics/claude-code)
- [repo — openai/codex: a harness that reads `.codex/skills`.](https://github.com/openai/codex)

[Part V](./README.md)
