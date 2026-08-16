# Ch 23 · Feature extraction（特征提取）

Turn the raw request into vectors / scalars **for other judges**. Features do not decide. One vector may feed relatedness, intent, and model router — **columns stay separate**.

## Concept

**Definition.** Features are measurements: length, language, fence ratio, paths, query embed, cosine to gist, turns since compact, cache hit, TPM. They are inputs. Relatedness, intent, and the model router each have their own head.

**Why it exists.** The hot path cannot wait for a second LLM. Milliseconds to one embed is the budget. Sharing *columns* is fine; fusing *decisions* is how fragments get mashed.

**Mechanism.** Surface rules. Symbol extract (regex / tree-sitter). Sentence vectors (E5, GTE, text-embedding-3, bge-m3). Cross: query–gist cosine, token Jaccard. Session counters. Gateway metrics. Structure (unclosed `tool_call`, plan mode).

**Boundaries.** An embedding is not a router. A cosine is not SHIFT. A second LLM classifier belongs on the model-router / intent service unless you pay for it on the context-layer hot path.

**Failure modes.** One softmax over “shift + intent + cheap-vs-strong.” Using a 8k-dim embed on every request then wondering about p99.

## Comparison

| Family | Examples | Typical algo |
| --- | --- | --- |
| Surface | length, language, fence ratio, `?`, command verbs | rules |
| Symbols | paths, repo, issue id, imports, function names | regex / tree-sitter |
| Sentence vectors | query embed, tail mean, gist embed | E5, GTE, text-embedding-3, bge-m3 |
| Cross | query–gist cosine, token Jaccard | dot product |
| Session | turn count, since compact, since model swap, idle | counters |
| Runtime | cache hit, remaining window, TPM, plan, tools present | gateway metrics |
| Structure | unclosed `tool_call`, plan mode | state machine |

**Read (读):**
- [paper — E5 (Wang et al.): weakly-supervised text embeddings still used as sentence vectors.](https://arxiv.org/abs/2212.03533)
- [paper — GTE (Li et al.): another widely served embedding family.](https://arxiv.org/abs/2308.03281)
- [docs — OpenAI embeddings: `text-embedding-3-*` as a hosted sentence vector.](https://platform.openai.com/docs/guides/embeddings)
- [docs — BAAI bge-m3: multilingual embed often used for query–gist cosine.](https://huggingface.co/BAAI/bge-m3)
- [repo — tree-sitter: the parser behind many “symbol” features (function names, not semantics).](https://github.com/tree-sitter/tree-sitter)
- [paper — Sentence-BERT (Reimers & Gurevych): the sentence-vector idea relatedness still borrows.](https://arxiv.org/abs/1908.10084)

[Part IV](./README.md)
