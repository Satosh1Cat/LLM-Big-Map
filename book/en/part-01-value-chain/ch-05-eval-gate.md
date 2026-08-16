# Ch 5 · Eval gate

The missing box between “we trained it” and “we can sell / buy it”.

## Concept

**Definition.** An eval gate is a *decision*: ship, buy, or change the harness. It is not a vibe and not a single IQ number. **Agent score = model + harness + tools + prompt + step budget.** Swap one piece, scores are not comparable. Public boards cover a narrow task mix.

**Why it exists.** Publishers need a ship gate. Buyers need a buy gate. Agent products need it more, because the hidden variable is the scaffold (bash-only vs an edit tool can move SWE-bench several to >10 points — that is [SOP](../part-05-agent-runtime/ch-26-coding-sop.md), not “IQ”).

**Mechanism.** Pick a task family. Freeze the harness. Run. Report the metric the bench defines (fail-to-pass ∧ keep-pass; Elo; composite). Needle-in-a-haystack is an *eval phenomenon* (recall vs length), not a paper to overweight. [Lost in the Middle](https://arxiv.org/abs/2307.03172) is the *position* effect inside a long window. Offline eval belongs on [Batch](../part-02-inference-cost/ch-09-batch-flex.md) (official 50% / ~24h), not on the interactive agent loop.

**Boundaries.** SWE-bench measures issue → patch → tests. τ-bench measures multi-turn tools + DB end state. Arena measures human pref, not code. A context layer needs *its own slices* (continue after compact; old constraints after a topic shift) — see [Ch 25](../part-05-agent-runtime/ch-25-agent-evals.md). Do not use ATIS intent accuracy as a coding-agent gate.

**Example.** SWE-bench Verified is 500 human-filtered GitHub issues. mini-SWE-agent (almost only bash) vs a scaffold with an edit tool is a harness delta, not a model delta.

**Failure modes.** Leaderboard shopping. Changing prompt and claiming a new model. Reporting needle-in-haystack as if it refuted Lost in the Middle. Running agent evals on Flex/Batch without recording which SKU you used.

## Comparison

| Bench | Measures | Size | Score |
| --- | --- | --- | --- |
| SWE-bench Verified | GitHub issue → patch passes tests | 500 human-filtered | fail-to-pass ∧ keep-pass |
| SWE-bench full / Lite | same family | ~2294 / 300 | same |
| LM Arena | human blind pref | live votes | Elo |
| Artificial Analysis | quality + speed + price | hosted endpoints | composite |
| HELM | multi-scenario | scenario set | per scenario |

**Read (读):**
- [paper — SWE-bench (Jimenez et al.): real GitHub issues; unit tests as oracle.](https://arxiv.org/abs/2310.06770)
- [repo — princeton-nlp/SWE-bench: dataset, harness, and evaluation code.](https://github.com/princeton-nlp/SWE-bench)
- [docs — swebench.com: Verified = 500 human-reviewed; live leaderboard.](https://www.swebench.com/)
- [paper — Lost in the Middle (Liu et al. 2023): U-shaped use of long context; middle is worst. Why “just buy 1M” fails.](https://arxiv.org/abs/2307.03172)
- [repo — Kamradt needle-in-a-haystack: recall-vs-length eval phenomenon, not a theory paper.](https://github.com/gkamradt/LLMTest_NeedleInAHaystack)
- [paper — HELM (Liang et al.): multi-scenario living eval, not a single coding number.](https://arxiv.org/abs/2211.09110)
- [docs — LMSYS Arena: human blind preference Elo.](https://lmarena.ai)
- [docs — Artificial Analysis: hosted-endpoint quality + speed + price composite.](https://artificialanalysis.ai)

[Part I](./README.md)
