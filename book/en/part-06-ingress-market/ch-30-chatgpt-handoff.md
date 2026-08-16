# Ch 30 · ChatGPT handoff

Handoff is **HANDOFF_INGEST**, not stuffing the tree into Cursor. ChatGPT is a message tree (branches, edits, regenerate). The agent wants the **spine**.

## Concept

**Definition.** Official export is a tree: `mapping[id].parent / children`, `content.parts[]`. The agent wants a linearized spine plus a T2 brief on L1 pinned, not a replayable tool trace — ChatGPT tool-call shape ≠ Cursor’s.

**Why it exists.** Users already did the thinking in ChatGPT. Copy-paste loses branches, canvases, constraints. Overnight Batch compact is allowed; a human staring at ingest is not a Batch job.

**Mechanism.** 1 Raw JSON (export or brittle in-page API). 2 Walk `current_node` to root (drop regenerate side branches). 3 Linearize `role: user/assistant`, merge parts, drop UI-only. 4 Denoise (T0/T1). 5 Compact the whole history as T2; Batch OK. Brief: goal / constraints / decided / open / artifacts. 6 Load brief → L1 pinned. The Cursor-class session *starts after this*.

**Boundaries.** Not path-3 browsing. Not CONTINUE of the ChatGPT KV (you never had it). Encrypted compact items from OpenAI still cannot move — the brief must be text.

**Failure modes.** Loading the tree as L3 live tail. Keeping “thanks” / “try again” as if they were constraints. Treating textdocs/canvas as tool results.

## Comparison — tree export vs linear agent brief

| Step | Job | Detail |
| --- | --- | --- |
| 1 Raw JSON | official export or in-page conversation API | tree: `mapping[id].parent / children`; `content.parts[]` |
| 2 Spine | walk `current_node` to root | drop regenerate side branches |
| 3 Linearize | `role: user/assistant`, merge parts, drop UI-only | canvas/textdocs packed aside |
| 4 Denoise | drop “thanks” / “try again”; keep decisions, constraints, code, filenames | T0/T1 |
| 5 Compact | whole history as T2; Batch OK | brief: goal / constraints / decided / open / artifacts |
| 6 Load | brief → L1 pinned | the Cursor-class session *starts after this* |

**Read (读):**
- [docs — ChatGPT export help: the ZIP / `conversations.json` tree, not linear API messages.](https://help.openai.com/en/articles/7260999-how-do-i-export-my-chatgpt-history-and-data)
- [docs — OpenAI compaction: T2 on the linearized spine; Batch if offline.](https://developers.openai.com/api/docs/guides/compaction)
- [docs — Anthropic compaction: the other T2 if the destination window is Claude.](https://platform.claude.com/docs/en/build-with-claude/compaction)
- [docs — OpenAI Batch: overnight ingest is the 50%/24h queue.](https://platform.openai.com/docs/guides/batch)
- [docs — Anthropic effective context engineering: just-in-time retrieve; the brief should point at files, not dump them.](https://www.anthropic.com/engineering/effective-context-engineering-for-ai-agents)
- [paper — Lost in the Middle: why stuffing the whole tree into the new window fails.](https://arxiv.org/abs/2307.03172)

[Part VI](./README.md)
