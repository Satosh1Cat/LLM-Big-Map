# Ch 25 · Agent evals

Score = model + harness + tools + prompt + step budget. Public boards are narrow.

## Concept

**Definition.** An agent eval measures a *system*, not a checkpoint. SWE-bench: issue → patch → tests. τ-bench: multi-turn tools + DB end state. AgentBench: OS / DB / web / puzzles. WebArena / WebVoyager / OSWorld: GUI. A context layer needs **its own slices**, not SWE-bench as the only number: (1) continue after compact; (2) old constraints after a topic shift; (3) foreign ChatGPT ingest then resume.

**Why it exists.** Harness deltas move scores several to >10 points. Buyers who compare “Claude vs GPT on SWE-bench” without naming the scaffold are comparing two different systems.

**Mechanism.** Freeze model, harness, tools, prompt, step budget. Run. Report the metric the bench defines. Batch is the right queue ([Ch 9](../part-02-inference-cost/ch-09-batch-flex.md)). CUA launch numbers (OpenAI, public snapshot): WebVoyager 87%, WebArena 58.1%, OSWorld 38.1% (human OSWorld 72.4%). Snapshot, not a law.

**Boundaries.** Public coding boards ≠ policy-following τ-bench ≠ GUI OSWorld. Internal evals: your SOP, repo, ACL — asserts + LLM-as-judge + audit.

**Failure modes.** Leaderboard shopping. Changing the edit tool and claiming a new model. Using interactive 1.0× pricing for a 10k-run overnight eval.

## Comparison

| Bench | Measures | Size | Score |
| --- | --- | --- | --- |
| SWE-bench Verified | issue → patch tests | 500 | fail-to-pass ∧ keep-pass |
| τ-bench / τ²-bench | multi-turn tools + DB end state | retail/airline etc. | policy + state |
| AgentBench | OS / DB / web / puzzles | 8 envs | per-env success |
| WebArena / WebVoyager / OSWorld | browser / desktop GUI | task sets | trajectory success |
| Internal | your SOP, repo, ACL | yours | asserts + LLM-as-judge + audit |

**Read (读):**
- [paper — SWE-bench (Jimenez et al.).](https://arxiv.org/abs/2310.06770)
- [repo — princeton-nlp/SWE-bench.](https://github.com/princeton-nlp/SWE-bench)
- [docs — swebench.com (Verified = 500).](https://www.swebench.com/)
- [paper — τ-bench (Yao / Sierra): tools + final DB state, not just a patch.](https://arxiv.org/abs/2406.12045)
- [repo — sierra-research/tau-bench.](https://github.com/sierra-research/tau-bench)
- [paper — AgentBench (Liu et al.): 8 environments, per-env success.](https://arxiv.org/abs/2308.03688)
- [repo — THUDM/AgentBench.](https://github.com/THUDM/AgentBench)
- [paper — WebArena (Zhou et al.): realistic web tasks.](https://arxiv.org/abs/2307.13854)
- [repo — web-arena-x/webarena.](https://github.com/web-arena-x/webarena)
- [paper — WebVoyager (He et al.): end-to-end web agents with vision.](https://arxiv.org/abs/2401.13919)
- [paper — OSWorld (Xie et al.): desktop GUI agents; human 72.4% as the reference.](https://arxiv.org/abs/2404.07972)
- [repo — xlang-ai/OSWorld.](https://github.com/xlang-ai/OSWorld)

[Part V](./README.md)
