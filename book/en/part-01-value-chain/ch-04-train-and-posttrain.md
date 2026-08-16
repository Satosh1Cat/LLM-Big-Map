# Ch 4 · Pretrain & post-train

Step 2 crushed two cost curves into one word.

## Concept

**Definition.** **Pretrain（预训练）** is next-token training on B1-scale raw tokens: megaclusters, rare cadence, the base artifact. **Post-train（后训练）** is SFT / DPO / GRPO / RLVR / RLAIF on prefs, synthetic traces, and verifiable rewards: still GPU-hour, orders of magnitude smaller, the cadence that actually ships a usable model. Safety / red team (C3) is a *ship gate*, not an appendix. Distill / open forks (C4) are a *product shape*, not another from-scratch run.

**Why it exists.** “We trained a model” in the press hides whether someone spent a pretrain or a weekend of DPO. Buyers who skip this split cannot tell a new base from a LoRA on someone else’s base.

**Mechanism.** C1 reads B1, writes a base checkpoint or a closed API. C2 reads B2/B3/B4-as-reward, writes the aligned / tool-using / domain-voiced model. Hosted LoRA (E3) is a *product* on top of someone else’s C1 — see [Ch 6](./ch-06-inference-api.md). Apps do not need C1. Product-level refusal/style can live in the harness.

**Boundaries.** Post-train is not “small pretrain.” RLVR (tests, math) is not the same supervision as DPO preference pairs. C3 (METR, ASL, Preparedness) is an *external* gate, not a training recipe. Open weights on Hugging Face / ModelScope are C4/C5, not hosted inference.

**Example.** A lab system card that splits “pretraining data mix” from “RLHF / constitutional” is doing this chapter in public. A Together / Fireworks LoRA job is E3, not C1.

**Failure modes.** Calling every checkpoint a “new model.” Treating safety as a blog post after launch. Distilling a teacher and then evaluating the student on the teacher’s own leaked eval.

## Comparison — pretrain vs post-train

| | C1 Pretrain | C2 Post-train |
| --- | --- | --- |
| Input | B1 raw tokens | B2 prefs, B3 synthetic, B4 as RLVR |
| Bill | GPU-hour at cluster scale | still GPU-hour, much smaller; or hosted FT jobs |
| Methods | next-token on web/code | SFT, DPO, GRPO, RLVR, RLAIF |
| Who | frontier labs | labs; Together etc. also sell FT |
| Cadence | rare, huge | frequent, ships the usable model |

| Method | Supervision | Typical use |
| --- | --- | --- |
| SFT | demonstrations | format, tools, domain voice |
| DPO | preference pairs | no separate reward model |
| GRPO / RLVR | verifiable reward (tests, math) | coding / reasoning |
| RLAIF | model-written prefs | cheaper than B2 |

**Read (读):**
- [paper — Attention Is All You Need (Vaswani et al.): Transformer baseline every C1 still sits on.](https://arxiv.org/abs/1706.03762)
- [paper — InstructGPT (Ouyang et al.): public post-train recipe — SFT then PPO-from-prefs; C2, not C1.](https://arxiv.org/abs/2203.02155)
- [paper — Direct Preference Optimization (Rafailov et al.): DPO, preference pairs without a separate reward model.](https://arxiv.org/abs/2305.18290)
- [paper — DeepSeek-R1: large-scale reasoning post-train with RL; a C2 narrative, not a chip paper.](https://arxiv.org/abs/2501.12948)
- [docs — METR: independent C3 evaluations of autonomous capability, not a training recipe.](https://metr.org)
- [docs — Anthropic Claude system card (current): how one lab splits pretrain / post-train / safety in public.](https://www.anthropic.com/claude)
- [docs — Hugging Face Hub: C4/C5 weight distribution, not hosted token meters.](https://huggingface.co/models)

[Part I](./README.md)
