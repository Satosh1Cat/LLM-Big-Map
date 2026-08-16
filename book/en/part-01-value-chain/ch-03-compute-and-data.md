# Ch 3 · Compute & data

Cost floor (chips + GPU cloud) and the data supply chain the 9-step sketch hid inside “Scale AI”.

## Concept

**Definition.** A1 chips and A2 GPU cloud are the physical floor: no cards, no train, no step 4/5. B1–B4 is the *data* chain: raw crawl, human labels/prefs, synthetic/distill, eval sets. Scale AI is **B2** — the most expensive, easiest-to-price slice — not “data.”

**Why it exists.** Product news talks about models. The bill starts earlier: who owns H100/B200 time, who licensed the crawl, who paid raters, who published the eval set you will be scored on.

**Mechanism.** Pretrain reads B1 (Common Crawl, FineWeb, licensed news, GitHub). Post-train reads B2 prefs and B3 teacher traces. Ship/buy gates read B4 (SWE-bench, MMLU, Arena). Human B2 is billed per hour / row / preference pair. Synthetic B3 is billed as teacher tokens + student GPU-hour. Eval sets are often free artifacts; human eval on top is paid.

**Boundaries.** B4 eval sets are both a data product and the input to [Ch 5](./ch-05-eval-gate.md). They are not a labeling company. App companies usually never touch A1; they buy [Ch 6](./ch-06-inference-api.md) tokens. Distill (B3) is not “we trained a new base model.”

**Example.** FineWeb is a filtered Common Crawl derivative you can actually name. SWE-bench is an eval set with unit tests as oracle — you do not pay Scale to “have SWE-bench,” you pay if you want extra human review or a private slice.

**Failure modes.** Calling all data work “RLHF.” Copying teacher bias into the student (B3) and calling it ground truth. Treating GPU-hour quotes as token prices (A2 vs E2).

## Comparison — Scale labeling vs synthetic / distill

| | B2 Human label / prefs | B3 Synthetic / distill |
| --- | --- | --- |
| Who | Scale, Surge, Mercor, Labelbox | Lab recipes; teacher GPT/Claude/DeepSeek |
| Bill | hour / row / preference pair | teacher tokens + student GPU-hour |
| Signal | RLHF prefs, expert traces | SFT from teacher, self-play trajectories |
| Failure | slow, pricey, rater drift | teacher bias copied into the student |

| Layer | Sellers | Bill |
| --- | --- | --- |
| A1 chips | NVIDIA, TPU, 昇腾, Groq LPU, Cerebras | card / system; often folded into token price |
| A2 GPU cloud | AWS, CoreWeave, 火山引擎, Aliyun | GPU-hour |
| B1 raw | Common Crawl, FineWeb, GitHub, licensed news | crawl + license |
| B4 eval | SWE-bench, MMLU, LM Arena, Scale SEAL | often free sets; paid human eval |

**Read (读):**
- [docs — Scale AI: B2 labeling / prefs / eval seller, not the crawl.](https://scale.com)
- [docs — FineWeb dataset card: Hugging Face’s public filtered web corpus (B1).](https://huggingface.co/datasets/HuggingFaceFW/fineweb)
- [docs — Common Crawl: the raw crawl FineWeb and most pretrain mixes still depend on.](https://commoncrawl.org/)
- [docs — SWE-bench: coding *eval set* (B4), not a labeling company.](https://www.swebench.com/)
- [repo — princeton-nlp/SWE-bench: original issues, tests, and harness code.](https://github.com/princeton-nlp/SWE-bench)
- [paper — Phi-1 (Gunasekar et al.): textbook-quality synthetic data as a B3 recipe, not a new chip.](https://arxiv.org/abs/2306.11644)
- [docs — NVIDIA product page for data-center GPUs: A1 as a catalog, not a token meter.](https://www.nvidia.com/en-us/data-center/products/)

[Part I](./README.md)
