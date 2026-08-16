# Ch 2 · 7 segments, 26 layers

The 9 boxes, split. Scan by **segment（段）**. Architect by **layer（层）**. “Required” depends on lab vs hoster vs app.

## Concept

**Definition.** An engineering panorama of the same industry the 9-step sketch named. 7 segments (compute, data, make-model, quality, inference, control, app), 26 layers. Order of the 9 steps is still right. The boxes were too fat: eval, post-train, observability, enterprise wrap, Agent runtime, and context/memory were missing; data / train / inference / app were each one smear.

**Why it exists.** Step 2 hid pretrain *and* post-train. Step 1 hid raw crawl vs human prefs vs synthetic vs eval sets. Step 8 hid the agent harness, the context layer, MCP/skills, and the product shape (Cursor vs ChatGPT vs Harvey). If you only have 9 boxes, every new firm looks like it “does AI.”

**Mechanism.** Read **left-to-right as money flow**, **down as specialization**. A1 chips and A2 GPU cloud are the cost floor. B1–B4 split “data.” C1–C5 split “make a model.” D1 is the public quality gate. E1–E5 split engine / hosted / FT / pricing SKUs / HTTP contract. F1–F4 are identity, traces, gateway, enterprise wrap. G1–G5 are runtime, context, tools, product shape, buyer. The context layer sits between **F3 gateway** and **G1/G2 runtime**.

**Boundaries.** “Required” is a role, not a moral. Frontier labs must own C1. An IDE app buys inference and *must* own G1 if it ships an agent; it does not pretrain. Gateway (F3) is optional if the app talks to one lab; required for multi-model, China relay, or enterprise single-exit.

**Example.** SWE-bench is B4 (eval set) *and* D1 (quality board) *and* later G1’s hidden SOP — one artifact, three layers. SiliconFlow is E1 eaten by E2+E5. Cursor is G4, with G1/G2/G3 inside the product, often skipping F3.

**Failure modes.** Merging G2 (what enters the window) with F3 (which model / which key). Calling every hoster a “gateway.” Treating open weights (C4/C5) as if they were a hosted token meter.

## Comparison — 9-step vs 26-layer

| ID | Segment | Layer | vs 9-step | Examples |
| --- | --- | --- | --- | --- |
| A1 | Compute | Chips | Hidden under 4/5 | H100/B200, TPU, 昇腾, Groq LPU |
| A2 | Compute | GPU cloud | Hidden | AWS, CoreWeave, 火山引擎 |
| B1 | Data | Raw corpus | Swallowed by 1 | Common Crawl, FineWeb, GitHub |
| B2 | Data | Human label / prefs | Step 1 (Scale) | Scale, Surge, Mercor |
| B3 | Data | Synthetic / distill | Missing | Phi/Qwen distill, teacher SFT |
| B4 | Data | Eval sets | Missing | SWE-bench, MMLU, LM Arena |
| C1 | Make model | Pretrain | Half of step 2 | OpenAI, Anthropic, DeepSeek, Qwen |
| C2 | Make model | Post-train (SFT/RL) | Merged into 2 | SFT, DPO, GRPO, RLVR |
| C3 | Make model | Safety / red team | Missing | ASL, Preparedness, METR |
| C4 | Make model | Distill / fork | Swallowed by 3 | Llama, Qwen, HF / ModelScope |
| C5 | Make model | Artifact | Step 3 | Closed API vs weight file |
| D1 | Quality | Benchmarks | Missing | LM Arena, SWE-bench, Artificial Analysis |
| E1 | Inference | Engine | Step 4 | vLLM, SGLang, TensorRT-LLM |
| E2 | Inference | Hosted | Step 5 | SiliconFlow, Fireworks, Bedrock, 百炼 |
| E3 | Inference | LoRA / FT product | Missing | OpenAI FT, Together, Fireworks LoRA |
| E4 | Inference | Pricing SKUs | Swallowed by 5 | Cache, Batch, Flex |
| E5 | Inference | API contract | Step 6 | Completions / Responses, Messages, Converse |
| F1 | Control | Identity / billing | Missing | org billing, WorkOS SSO |
| F2 | Control | Observability | Missing | LangSmith, Helicone, Langfuse |
| F3 | Control | Gateway / Router | Step 7 | OpenRouter, LiteLLM, one-api |
| F4 | Control | Enterprise wrap | Missing | VPC, 等保, DLP |
| G1 | App | Agent runtime / harness | Swallowed by 8 | tool loop, MCP, sandbox |
| G2 | App | Context / memory | Missing (7↔8) | compaction, repo index |
| G3 | App | Tools / MCP / Skills | Missing | MCP registry, skills |
| G4 | App | Product by shape | Step 8 too coarse | Cursor vs ChatGPT vs Perplexity vs Harvey |
| G5 | App | User / buyer | Step 9 | personal sub vs enterprise seat |

**Required vs optional:**

| Layer | Frontier lab | Open weights + self-host | App (IDE) |
| --- | --- | --- | --- |
| Chips / cloud | required | required | buy inference |
| Pretrain | required | required or fork | no |
| Post-train / safety | required | common | optional |
| Eval | ship gate | ship gate | **buy** gate |
| Engine | own serving | if self-host | no |
| Hosted + API | they sell it | sell or use | required if not self-host |
| Gateway | they *are* the door | optional | optional if direct; required if multi-model/relay |
| Observability | built-in | common | prod ≈ required |
| Agent runtime | if they ship Chat | depends | required for Agent apps |
| VPC / compliance | big accounts | ToB | ToB ≈ required |

Drill [Ch 3](./ch-03-compute-and-data.md)–[Ch 7](./ch-07-control-plane.md). G2: [Part III](../part-03-context-router/README.md). E4: [Part II](../part-02-inference-cost/README.md).

**Read (读):**
- [docs — Hugging Face FineWeb: a public pretrain corpus (B1), what “data” looks like when it is not Scale labeling.](https://huggingface.co/datasets/HuggingFaceFW/fineweb)
- [docs — Common Crawl: the raw web crawl most pretrain mixes still start from (B1).](https://commoncrawl.org/)
- [paper — SWE-bench (Jimenez et al.): GitHub issues + tests as oracle; this index’s canonical B4/D1 coding eval.](https://arxiv.org/abs/2310.06770)
- [docs — SWE-bench site: Verified = 500 human-reviewed items; leaderboard, not a labeling company.](https://www.swebench.com/)
- [docs — LMSYS Chatbot Arena: live human preference Elo; a D1 board that is not SWE-bench.](https://lmarena.ai)
- [repo — vLLM engine (E1) again, because the 26-layer map refuses to merge engine with hosted inference.](https://github.com/vllm-project/vllm)
- [docs — Model Context Protocol: G3 tool protocol, not a gateway product.](https://modelcontextprotocol.io)

[Part I](./README.md)
