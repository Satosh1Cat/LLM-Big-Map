# Ch 26 · Coding agent SOP

SOP = a repeatable step sequence, not weights. In coding agents it is the harness **outer loop**.

## Concept

**Definition.** Locate → plan → patch → close. SWE-bench’s hidden SOP: repo at a commit + issue text + step budget → unified diff, scored by tests. Different scaffolds (bash-only vs edit tools) move the score several to >10 points — SOP, not IQ. Version SOP changes; they change evals.

**Why it exists.** The model is not the product. The loop that searches, edits, runs tests, and stops is. Skills vs rules vs main loop: [Ch 27](./ch-27-skills-auto-sop.md). Public harnesses: Codex CLI, Claude Code, Gemini CLI, opencode, OpenHands.

**Mechanism.** Carriers: system-prompt steps, hooks (before/after tools), rule files, Skills. mini-SWE-agent shows how little tool surface still scores. After compact, re-read recent files (Codex ≤5) is part of the SOP, not a separate “memory product.”

**Boundaries.** SOP is not post-train. Changing the SOP and keeping the same model is a new system. Do not dump every SOP into system (L0 bloat).

**Failure modes.** Unversioned prompt drift between eval runs. Infinite repair loops with no stop rule. Treating OpenHands and mini-SWE-agent scores as the same SWE-bench.

## Comparison

| Phase | Common steps |
| --- | --- |
| Locate | read issue → search symbols → open files → write task bounds |
| Plan | file-level plan; some products start in plan mode |
| Patch | edit → linter → tests → re-read failures |
| Close | diff self-check → commit message → stop |

**Read (读):**
- [paper — SWE-bench: the oracle is tests; the hidden variable is the scaffold.](https://arxiv.org/abs/2310.06770)
- [repo — princeton-nlp/SWE-bench.](https://github.com/princeton-nlp/SWE-bench)
- [repo — SWE-agent / mini-SWE-agent (SWE-bench team): how little tool surface still scores.](https://github.com/SWE-agent/mini-swe-agent)
- [repo — openai/codex: Codex CLI outer loop.](https://github.com/openai/codex)
- [repo — anthropics/claude-code: Claude Code outer loop.](https://github.com/anthropics/claude-code)
- [repo — google-gemini/gemini-cli.](https://github.com/google-gemini/gemini-cli)
- [repo — anomalyco/opencode.](https://github.com/anomalyco/opencode)
- [repo — All-Hands-AI/OpenHands: open-source coding agent runtime.](https://github.com/All-Hands-AI/OpenHands)

[Part V](./README.md)
