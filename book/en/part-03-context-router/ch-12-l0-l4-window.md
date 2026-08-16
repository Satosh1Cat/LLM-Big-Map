# Ch 12 · L0–L4 window

Five layers from stable to volatile so [prompt cache](../part-02-inference-cost/ch-08-prompt-cache.md) has a prefix to hit, and compacting only punches the right layer.

## Concept

**Definition.** A packing discipline, not a vendor. L0 protocol prefix (tools schema, system, skill **name+description** only) must be byte-stable. L1 pinned (user rules, repo map, identity digest, handoff brief) hits cache if unchanged. L2 memory (compaction block / gist / Mamba→text) misses after every compact. L3 live tail (last N turns) grows each turn. L4 query (current utterance) is never cached.

**Why it exists.** Cache APIs *reward* clients who keep L0 byte-stable (Anthropic **0.1×** read). Compacting that rewrites L0 fights the bill. Putting full Skill bodies or exploding tool schemas in L0 busts the prefix.

**Mechanism.** CONTINUE: append L3/L4 only. COMPACT: rewrite L2, leave L0. RESET: clear L2–L3, keep L0 (+ some L1). Model swap: rebuild L0 in the new vendor dialect; old cache and encrypted blobs die. OpenAI implicit breakpoint sits on the latest user/tool — a timestamp in the prefix misses. Anthropic allows 4 explicit breakpoints; automatic mode *moves* the breakpoint toward the tail (not the same as a frozen L0 cut).

**Boundaries.** L0 stability is not “write a better prompt.” It is a cost constraint. “Rewrite system from intent every turn” is an intent-layer habit that destroys L0. Encrypted OpenAI compact items live in L2 and **cannot** move vendors.

**Example.** Cursor-style: discover less, fetch lazy, tools write files not inline — prefix stays hot. Codex at **90%** of window rewrites L2 via Responses compact, then re-reads ≤5 recent files (filesystem as L2 overflow).

**Failure modes.** Skill body in L0. Tool-result log bombs never cleared (Anthropic `clear_tool_uses` exists for this). `truncation: auto` dropping L3 and breaking the hash.

## Comparison

| Layer | Contents | Cache | Who writes |
| --- | --- | --- | --- |
| L0 protocol prefix | tools schema, system, skill **name+description** only | must be byte-stable | product / MCP |
| L1 pinned | user rules, repo map, identity digest, handoff brief | hits if unchanged | user + ingest |
| L2 memory | compaction block / gist / Mamba→text | miss after every compact | compact pipeline |
| L3 live tail | last N user/assistant/tool | grows each turn | session |
| L4 query | current utterance | never cached | user |

**Read (读):**
- [docs — OpenAI prompt caching: implicit breakpoint on latest user/tool; timestamps in the prefix miss.](https://developers.openai.com/api/docs/guides/prompt-caching)
- [docs — Anthropic prompt caching: 4 explicit breakpoints; automatic mode moves the cut toward the tail.](https://platform.claude.com/docs/en/build-with-claude/prompt-caching)
- [docs — Anthropic compaction: the readable block that occupies L2 and must be sent back.](https://platform.claude.com/docs/en/build-with-claude/compaction)
- [docs — OpenAI compaction: opaque L2 item; pass the returned window as-is.](https://developers.openai.com/api/docs/guides/compaction)
- [docs — Anthropic effective context engineering: smallest high-signal set; progressive disclosure of tools/skills.](https://www.anthropic.com/engineering/effective-context-engineering-for-ai-agents)
- [docs — Claude cookbook: tool clearing vs compact vs memory — which layer each lever punches.](https://platform.claude.com/cookbook/tool-use-context-engineering-context-engineering-tools)
- [docs — Agent Skills spec: `SKILL.md` metadata always loaded, body on demand — that split *is* L0 vs later layers.](https://agentskills.io/specification)

[Part III](./README.md)
