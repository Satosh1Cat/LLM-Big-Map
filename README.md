# llm-Big-Map
The Fog Around LLMs

After a year of working with LLMs, I still don't feel like I've built any real, systematic knowledge. Even when I've gone deep on the latest techniques in a specific area — RAG, for instance — and tracked every detail, I don't feel like the fog around LLMs as a whole has lifted. If anything, it feels just as thick.

Most beginners form their understanding of LLMs through news headlines and whatever project suddenly jumps into the spotlight. What's missing is systematic knowledge — but the institutions that could provide that kind of education simply can't keep pace with how fast the field moves. SwitchCraft launched on May 8, 2026, for example, and no textbook will ever record it. Information is everywhere, but precisely because it's scattered across so many places, people without an internal index to organize it struggle to actually get better as they consume more of it. Often, after months of "learning," there's more fog in front of them, not less.

This document is my attempt at building that missing index — a map of the pipeline from raw data to the person using the product, with definitions and current references at each stage.

The Pipeline
0. Background / History

A brief primer for readers new to the space: how the modern LLM stack came together, from early scaling-law research to today's agentic tooling.

↓

1. Data

Definition: the raw material for training — text, code, images, human feedback — collected, cleaned, and labeled at scale. Modern data pipelines increasingly include preference ranking and rubric scoring of model outputs (the fuel for RLHF), not just classic annotation.

↓

2. Train the Model

Definition: the process of updating model weights on that data — pretraining (learning general language patterns from huge corpora) followed by post-training steps like SFT (supervised fine-tuning) and RLHF (reinforcement learning from human feedback) to align the model with instructions and preferences.

↓

3. Get an LLM

Definition: the output of training — a set of weights implementing a transformer architecture, capable of next-token prediction at scale.

↓

4. Deploy the LLM

Definition: packaging the trained weights (quantization, sharding, checkpoint conversion) so they can run efficiently on serving hardware.

↓

5. Inference

References: vLLM · SGLang

Definition: the runtime engine that actually generates tokens from a deployed model, optimizing for throughput and latency under real traffic.

vLLM — introduced PagedAttention, borrowing OS-style paged memory management to make KV-cache handling far more memory-efficient. Paper: "Efficient Memory Management for Large Language Model Serving with PagedAttention" (Kwon et al., SOSP 2023).
SGLang — introduced RadixAttention, which reuses KV-cache across requests when prefixes match, cutting prefill computation. Paper: "SGLang: Efficient Execution of Structured Language Model Programs" (Zheng et al.).

↓

6. API

Definition: the standardized HTTP interface (most commonly the OpenAI-compatible chat/completions format) that lets any application talk to any inference backend without custom integration code.

↓

7. Gateway / Router

References: LiteLLM · RouteLLM (LMSYS)

Definition: a proxy layer sitting between applications and model providers. A gateway unifies many providers behind one API (key management, caching, fallback, cost tracking); a router goes further and picks the best or cheapest model per request based on task classification.

LiteLLM — the most widely adopted open-source, self-hosted gateway; normalizes 100+ providers behind one OpenAI-format endpoint.
RouteLLM — LMSYS's research-grade routing classifier for matching queries to the cheapest capable model.

↓

8. Application

Definition: the product layer built on top of the API/gateway — where model capability turns into a usable tool, whether that's a code editor, a chat interface, or an agentic workflow app.

↓

9. The User

The person whose problem the entire stack — from raw data to a rendered response — ultimately exists to solve.
