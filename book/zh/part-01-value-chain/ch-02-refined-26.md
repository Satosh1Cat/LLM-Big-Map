# 第 2 章 · 7 段 26 层

把 9 个格子拆开。用**段**扫行业，用**层**做架构。「必做」取决于你是实验室、托管商，还是应用。

## 概念

**定义。** 同一行业的工程全景。7 段（算力、数据、做模型、质量、推理、控制、应用），26 层。9 步顺序仍然对。格子太肥：评测、后训练、可观测性、企业封装、Agent runtime、上下文/记忆都缺；数据 / 训练 / 推理 / 应用各自糊成一块。

**为什么存在。** 第 2 步把预训练和后训练压在一起。第 1 步把原始爬取、人工偏好、合成数据、评测集糊在一起。第 8 步把 harness、上下文层、MCP/skills、产品形态（Cursor vs ChatGPT vs Harvey）全吞了。只有 9 格时，每家新公司看起来都「在做 AI」。

**机制。** **从左到右是钱**，**往下是分工**。A1 芯片和 A2 GPU 云是成本地板。B1–B4 拆「数据」。C1–C5 拆「做出一个模型」。D1 是公开质量门。E1–E5 拆引擎 / 托管 / 微调 / 计价 SKU / HTTP 契约。F1–F4 是身份、traces、Gateway、企业封装。G1–G5 是 runtime、上下文、工具、产品形态、买方。上下文层夹在 **F3 Gateway** 和 **G1/G2 runtime** 之间。

**边界。** 「必做」是角色，不是道德。前沿实验室必须自己做 C1。IDE 应用买推理；若出荷 agent 就必须有 G1，但不用预训练。Gateway（F3）在只连一家实验室时可选；多模型、中国中转、企业统一出口时才刚需。

**例子。** SWE-bench 同时是 B4（评测集）、D1（质量榜）、以及后来 G1 的隐藏 SOP——一份产物，三层。硅基流动是 E1 被 E2+E5 吃掉。Cursor 是 G4，G1/G2/G3 做在产品里，常常跳过 F3。

**失败模式。** 把 G2（什么进窗口）和 F3（哪颗模型 / 哪把钥匙）合成一个「智能路由」。把每家托管商都叫 Gateway。把开源权重（C4/C5）当成托管 token 计量表。

## 对比 — 9 步 vs 26 层

| ID | 段 | 层 | 相对 9 步 | 例子 |
| --- | --- | --- | --- | --- |
| A1 | 算力 | 芯片 | 藏在 4/5 下 | H100/B200, TPU, 昇腾, Groq LPU |
| A2 | 算力 | GPU 云 | 隐藏 | AWS, CoreWeave, 火山引擎 |
| B1 | 数据 | 原始语料 | 被 1 吞 | Common Crawl, FineWeb, GitHub |
| B2 | 数据 | 人工标注 / 偏好 | 第 1 步（Scale） | Scale, Surge, Mercor |
| B3 | 数据 | 合成 / 蒸馏 | 缺失 | Phi/Qwen distill, teacher SFT |
| B4 | 数据 | 评测集 | 缺失 | SWE-bench, MMLU, LM Arena |
| C1 | 做模型 | 预训练 | 第 2 步的一半 | OpenAI, Anthropic, DeepSeek, Qwen |
| C2 | 做模型 | 后训练（SFT/RL） | 并进 2 | SFT, DPO, GRPO, RLVR |
| C3 | 做模型 | 安全 / 红队 | 缺失 | ASL, Preparedness, METR |
| C4 | 做模型 | 蒸馏 / fork | 被 3 吞 | Llama, Qwen, HF / ModelScope |
| C5 | 做模型 | 产物 | 第 3 步 | 闭源 API vs 权重文件 |
| D1 | 质量 | Benchmarks | 缺失 | LM Arena, SWE-bench, Artificial Analysis |
| E1 | 推理 | 引擎 | 第 4 步 | vLLM, SGLang, TensorRT-LLM |
| E2 | 推理 | 托管 | 第 5 步 | 硅基流动, Fireworks, Bedrock, 百炼 |
| E3 | 推理 | LoRA / FT 产品 | 缺失 | OpenAI FT, Together, Fireworks LoRA |
| E4 | 推理 | 计价 SKU | 被 5 吞 | Cache, Batch, Flex |
| E5 | 推理 | API 契约 | 第 6 步 | Completions / Responses, Messages, Converse |
| F1 | 控制 | 身份 / 账单 | 缺失 | org billing, WorkOS SSO |
| F2 | 控制 | 可观测性 | 缺失 | LangSmith, Helicone, Langfuse |
| F3 | 控制 | Gateway / Router | 第 7 步 | OpenRouter, LiteLLM, one-api |
| F4 | 控制 | 企业封装 | 缺失 | VPC, 等保, DLP |
| G1 | 应用 | Agent runtime / harness | 被 8 吞 | tool loop, MCP, sandbox |
| G2 | 应用 | 上下文 / 记忆 | 缺失（7↔8） | compaction, repo index |
| G3 | 应用 | 工具 / MCP / Skills | 缺失 | MCP registry, skills |
| G4 | 应用 | 按形态分的产品 | 第 8 步过粗 | Cursor vs ChatGPT vs Perplexity vs Harvey |
| G5 | 应用 | 用户 / 买方 | 第 9 步 | 个人订阅 vs 企业座位 |

**必做 vs 可选：**

| 层 | 前沿实验室 | 开源权重 + 自托管 | 应用（IDE） |
| --- | --- | --- | --- |
| 芯片 / 云 | 必做 | 必做 | 买推理 |
| 预训练 | 必做 | 必做或 fork | 不做 |
| 后训练 / 安全 | 必做 | 常见 | 可选 |
| 评测 | 出荷门 | 出荷门 | **买**门 |
| 引擎 | 自己 serving | 若自托管 | 不做 |
| 托管 + API | 他们在卖 | 卖或用 | 不自托管则必买 |
| Gateway | 他们*就是*门 | 可选 | 直连则可选；多模型/中转则必做 |
| 可观测性 | 内建 | 常见 | 生产 ≈ 必做 |
| Agent runtime | 若出荷 Chat | 看情况 | Agent 应用必做 |
| VPC / 合规 | 大客户 | ToB | ToB ≈ 必做 |

往下拆 [第 3 章](./ch-03-compute-and-data.md)–[第 7 章](./ch-07-control-plane.md)。G2：[卷三](../part-03-context-router/README.md)。E4：[卷二](../part-02-inference-cost/README.md)。

**Read (读):**
- [docs — Hugging Face FineWeb：公开预训练语料（B1），「数据」不是 Scale 标注时长这样的样子。](https://huggingface.co/datasets/HuggingFaceFW/fineweb)
- [docs — Common Crawl：多数预训练配比仍会用到的原始网页爬取（B1）。](https://commoncrawl.org/)
- [paper — SWE-bench（Jimenez et al.）：GitHub issue + 测试当 oracle；本索引的 canonical B4/D1 编程评测。](https://arxiv.org/abs/2310.06770)
- [docs — SWE-bench 站点：Verified = 500 条人工审过；是榜，不是标注公司。](https://www.swebench.com/)
- [docs — LMSYS Chatbot Arena：真人盲选 Elo；另一块 D1 榜，不是 SWE-bench。](https://lmarena.ai)
- [repo — vLLM 引擎（E1）再出现一次：26 层图拒绝把引擎和托管推理并成一格。](https://github.com/vllm-project/vllm)
- [docs — Model Context Protocol：G3 工具协议，不是 Gateway 产品。](https://modelcontextprotocol.io)

[卷一](./README.md)
