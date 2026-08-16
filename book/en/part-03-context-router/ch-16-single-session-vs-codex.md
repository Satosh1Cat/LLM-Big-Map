# Ch 16 · One session vs Codex threads

User-visible object: one chat vs many task threads. Compaction policy follows the object.

## Concept

**Definition.** A **single-session** product shows one continuous chat. Shift detection must be good or auth constraints leak into a CSS task. Benefit: the user does not manage threads; L0 (tools/system) can stay hot all evening. **Codex-style threads** are per-task (desktop often per project). After compact, Codex re-reads up to 5 recently edited files — filesystem as external memory; same idea as lazy retrieve, different harness.

**Why it exists.** UX chooses the object; compacting is downstream. FORK is explicit in a one-chat UX. Codex can just open a thread.

**Mechanism.** Single-session: compact or stuff less **in the same line**; RESET or COMPACT in-line on topic shift. Codex: auto compact at **90%** of window via Responses compact; OpenAI path encrypted; local path handwritten handoff; thread often glued to one model (swap ≈ new context); multi-thread is first-class.

**Boundaries.** Session object ≠ relatedness model ≠ vendor compact API. Cursor-class lazy file retrieve is not Codex’s 90% blob. Model swap inside one chat kills cache and encrypted items; Codex often avoids that by gluing a thread to a model.

**Example.** User says “now do the CSS.” One-session must RESET_PREFIX or old auth notes pollute. Codex user opens another thread; the old one freezes.

**Failure modes.** Compacting instead of RESET on a true shift. Moving an OpenAI encrypted compact item into a Claude thread. Letting one-session inherit slots across RESET.

## Comparison

| Dimension | Single-session product | Codex |
| --- | --- | --- |
| User object | one continuous chat | threads per task; desktop often per project |
| Full window | compact or stuff less, **in the same line** | **90%** auto compact via Responses compact |
| Summary shape | readable or self-managed | OpenAI path encrypted; local path handwritten handoff |
| Model swap | user/router inside the same line | thread often glued to one model; swap ≈ new context |
| Parallel | sub-agent aside; main chat stays one | multi-thread is first-class |
| Topic shift | must RESET or COMPACT in-line or old constraints pollute | open a new thread; old one freezes |

**Read (读):**
- [docs — OpenAI compaction: what Codex’s 90% path returns (encrypted item; do not prune).](https://developers.openai.com/api/docs/guides/compaction)
- [repo — openai/codex: the public CLI whose session object is threads + 90% compact.](https://github.com/openai/codex)
- [docs — Anthropic effective context engineering: Claude Code compact + re-read recently accessed files after summary.](https://www.anthropic.com/engineering/effective-context-engineering-for-ai-agents)
- [repo — anthropics/claude-code: one-line session harness with `/compact`.](https://github.com/anthropics/claude-code)
- [repo — google-gemini/gemini-cli: 50% compact on a 1M window — another session object, another threshold.](https://github.com/google-gemini/gemini-cli)
- [repo — All-Hands-AI/OpenHands: open-source agent runtime with its own session/thread story.](https://github.com/All-Hands-AI/OpenHands)

[Part III](./README.md)
