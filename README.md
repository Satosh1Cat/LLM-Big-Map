# LLM Field Index（领域索引）

A living bilingual **field index**: each topic is a detailed concept, then other people's papers / repos / official docs.

This repo is concepts + curated primary links. PRs should add sources, not essays.

- **English:** [`book/en/README.md`](./book/en/README.md) — jargon glossed as `compaction (压缩)`
- **中文:** [`book/zh/README.md`](./book/zh/README.md) — 专有名词保留英文

Same TOC, same filenames, 1:1. License: [CC-BY-4.0](./LICENSE) (text). How to help: [CONTRIBUTING.md](./CONTRIBUTING.md).

This is an industry field guide, not a product manual. It does not document any vendor's unreleased architecture.

---

## 前言 / Preface

[中文全文](./book/zh/00-preface.md) · [English](./book/en/00-preface.md)

在今天，LLM已经激活了整个世界上科技业的繁荣生态，他成了局内人的盛宴。几个月的时间足以完成一次技术迭代。但对于一个初学者，尤其是初学计算机技术的人来说，llm越来越陌生不可测了，大多数初学者对行业迭代信息的了解来源于新闻或者某些新的产品的单独介绍，这种信息带来的知识，绝对是不足以帮助一位初入行业的人拥有体系化的认识与足够快，足够完整的了解这个行业，大多数人只是迷茫在入口处，不断看到新的产品，新的进化，而越来越觉得LLM神秘莫测。

本人同样作为初学者，打算在学习的过程中完成这本书。

Today LLMs have switched on a booming tech ecology worldwide. It has become a feast for insiders. A few months is enough for a full technical generation. For a beginner — especially someone new to computing — LLMs feel more foreign and unreadable by the month. Most beginners learn the industry from news, or from isolated write-ups of a new product. That kind of knowledge is not enough for a systematic picture, nor for a fast and complete view of the field. Most people stay lost at the entrance: they keep seeing new products and new evolutions, and LLM feels more mysterious every time.

I am a beginner too. I intend to finish this book while I am still learning.

---

## How to read（怎么读）

Do not linear-read 1→31. Start with [Preface / 前言](./book/zh/00-preface.md).

| Path | Time | Question | Start |
| --- | --- | --- | --- |
| **A · Elevator** | 30 min | Where do firms and money sit? | [EN Ch 1](./book/en/part-01-value-chain/ch-01-nine-steps.md) · [中文](./book/zh/part-01-value-chain/ch-01-nine-steps.md) |
| **B · Gaps** | 2 h | What did the 9-step sketch squash? | [EN Ch 2](./book/en/part-01-value-chain/ch-02-refined-26.md) · [中文](./book/zh/part-01-value-chain/ch-02-refined-26.md) |
| **C · Context layer** | half day | What should this agent see *now*? | [EN Ch 11](./book/en/part-03-context-router/ch-11-definition.md) · [中文](./book/zh/part-03-context-router/ch-11-definition.md) |

**A — leave with:** 9-step order is right, boxes too fat; SiliconFlow-class firms sell engine + hosted inference + API as one SKU（库存单元）; Gateway is optional if the app talks to Anthropic/OpenAI directly.

**C — keep fragments split:** relatedness ≠ compacting ≠ intent ≠ model router.

Each chapter: detailed concept (what / why / how / boundaries / example / failure) · comparison · **Read (读)** primary links.

---

## Contents（目录）

Numbering is identical in `book/en/` and `book/zh/`.

| Ch | EN | 中文 |
| --- | --- | --- |
| 0 | [Preface](./book/en/00-preface.md) | [前言](./book/zh/00-preface.md) |

### Part I · Value chain（价值链）

| Ch | EN | 中文 |
| --- | --- | --- |
| 1 | [9-step sketch](./book/en/part-01-value-chain/ch-01-nine-steps.md) | [9 步草图](./book/zh/part-01-value-chain/ch-01-nine-steps.md) |
| 2 | [7×26 map](./book/en/part-01-value-chain/ch-02-refined-26.md) | [7 段 26 层](./book/zh/part-01-value-chain/ch-02-refined-26.md) |
| 3 | [Compute & data](./book/en/part-01-value-chain/ch-03-compute-and-data.md) | [算力与数据](./book/zh/part-01-value-chain/ch-03-compute-and-data.md) |
| 4 | [Pretrain & post-train](./book/en/part-01-value-chain/ch-04-train-and-posttrain.md) | [预训练与后训练](./book/zh/part-01-value-chain/ch-04-train-and-posttrain.md) |
| 5 | [Eval gate](./book/en/part-01-value-chain/ch-05-eval-gate.md) | [评测门](./book/zh/part-01-value-chain/ch-05-eval-gate.md) |
| 6 | [Inference & API](./book/en/part-01-value-chain/ch-06-inference-api.md) | [推理与 API](./book/zh/part-01-value-chain/ch-06-inference-api.md) |
| 7 | [Control plane](./book/en/part-01-value-chain/ch-07-control-plane.md) | [控制面](./book/zh/part-01-value-chain/ch-07-control-plane.md) |

### Part II · Inference cost（推理成本）

| Ch | EN | 中文 |
| --- | --- | --- |
| 8 | [Prompt cache](./book/en/part-02-inference-cost/ch-08-prompt-cache.md) | [Prompt cache](./book/zh/part-02-inference-cost/ch-08-prompt-cache.md) |
| 9 | [Batch & Flex](./book/en/part-02-inference-cost/ch-09-batch-flex.md) | [Batch 与 Flex](./book/zh/part-02-inference-cost/ch-09-batch-flex.md) |
| 10 | [Relay & discount](./book/en/part-02-inference-cost/ch-10-relay-discount.md) | [中转站与折扣](./book/zh/part-02-inference-cost/ch-10-relay-discount.md) |

### Part III · Context layer（上下文层）

| Ch | EN | 中文 |
| --- | --- | --- |
| 11 | [Definition & actions](./book/en/part-03-context-router/ch-11-definition.md) | [定义与动作](./book/zh/part-03-context-router/ch-11-definition.md) |
| 12 | [L0–L4 window](./book/en/part-03-context-router/ch-12-l0-l4-window.md) | [L0–L4 窗口](./book/zh/part-03-context-router/ch-12-l0-l4-window.md) |
| 13 | [A typical pipeline](./book/en/part-03-context-router/ch-13-pipeline.md) | [典型管线](./book/zh/part-03-context-router/ch-13-pipeline.md) |
| 14 | [Compacting](./book/en/part-03-context-router/ch-14-compacting.md) | [Compacting](./book/zh/part-03-context-router/ch-14-compacting.md) |
| 15 | [Relatedness](./book/en/part-03-context-router/ch-15-relatedness.md) | [相关](./book/zh/part-03-context-router/ch-15-relatedness.md) |
| 16 | [One session vs Codex](./book/en/part-03-context-router/ch-16-single-session-vs-codex.md) | [单 session vs Codex](./book/zh/part-03-context-router/ch-16-single-session-vs-codex.md) |
| 17 | [Mamba / RNN](./book/en/part-03-context-router/ch-17-mamba-rnn.md) | [Mamba / RNN](./book/zh/part-03-context-router/ch-17-mamba-rnn.md) |

### Part IV · Routing & NLU（路由与理解） — keep separate

| Ch | EN | 中文 |
| --- | --- | --- |
| 18 | [Model Router](./book/en/part-04-routing-nlu/ch-18-model-router.md) | [Model Router](./book/zh/part-04-routing-nlu/ch-18-model-router.md) |
| 19 | [API switching](./book/en/part-04-routing-nlu/ch-19-api-switching.md) | [API 切换](./book/zh/part-04-routing-nlu/ch-19-api-switching.md) |
| 20 | [Intent](./book/en/part-04-routing-nlu/ch-20-intent.md) | [意图](./book/zh/part-04-routing-nlu/ch-20-intent.md) |
| 21 | [Slot filling](./book/en/part-04-routing-nlu/ch-21-slot-filling.md) | [Slot filling](./book/zh/part-04-routing-nlu/ch-21-slot-filling.md) |
| 22 | [Context engineering](./book/en/part-04-routing-nlu/ch-22-context-engineering.md) | [上下文工程](./book/zh/part-04-routing-nlu/ch-22-context-engineering.md) |
| 23 | [Feature extraction](./book/en/part-04-routing-nlu/ch-23-feature-extraction.md) | [特征提取](./book/zh/part-04-routing-nlu/ch-23-feature-extraction.md) |

### Part V · Agent runtime（运行时）

| Ch | EN | 中文 |
| --- | --- | --- |
| 24 | [Permission / Identity](./book/en/part-05-agent-runtime/ch-24-permission-identity.md) | [Permission / Identity](./book/zh/part-05-agent-runtime/ch-24-permission-identity.md) |
| 25 | [Agent evals](./book/en/part-05-agent-runtime/ch-25-agent-evals.md) | [Agent evals](./book/zh/part-05-agent-runtime/ch-25-agent-evals.md) |
| 26 | [Coding SOP](./book/en/part-05-agent-runtime/ch-26-coding-sop.md) | [Coding SOP](./book/zh/part-05-agent-runtime/ch-26-coding-sop.md) |
| 27 | [Skills / auto-SOP](./book/en/part-05-agent-runtime/ch-27-skills-auto-sop.md) | [Skills / 自动 SOP](./book/zh/part-05-agent-runtime/ch-27-skills-auto-sop.md) |
| 28 | [Browser agent](./book/en/part-05-agent-runtime/ch-28-browser-general-agent.md) | [浏览器 agent](./book/zh/part-05-agent-runtime/ch-28-browser-general-agent.md) |

### Part VI · Ingress & market（入口与市场）

| Ch | EN | 中文 |
| --- | --- | --- |
| 29 | [Web-to-Agent](./book/en/part-06-ingress-market/ch-29-web-to-agent.md) | [Web-to-Agent](./book/zh/part-06-ingress-market/ch-29-web-to-agent.md) |
| 30 | [ChatGPT handoff](./book/en/part-06-ingress-market/ch-30-chatgpt-handoff.md) | [ChatGPT handoff](./book/zh/part-06-ingress-market/ch-30-chatgpt-handoff.md) |
| 31 | [Marketplace](./book/en/part-06-ingress-market/ch-31-marketplace.md) | [Marketplace](./book/zh/part-06-ingress-market/ch-31-marketplace.md) |

Bibliography: [EN](./book/en/bibliography.md) · [中文](./book/zh/bibliography.md)

---

## Correct a fact（纠错）

Open an issue (`fact-error`) or a PR against **one chapter**. Numbers (cache 0.1×, Batch 50%, Codex 90%) need an official URL + date. See [CONTRIBUTING.md](./CONTRIBUTING.md).
