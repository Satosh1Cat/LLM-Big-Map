# Ch 14 · Compacting（压缩）

Lossy operation on the **whole** transcript (user / assistant / tool). Goal: keep the working set where the model can still attend. Long windows hit lost-in-the-middle (Liu et al. 2023).

## Concept

**Definition.** Compacting is *lossy compression of the current window*, not infinite context, not RAG, not relatedness. T0/T1 can run without an LLM (drop thanks, keep decisions). T2 is a vendor or LLM summary. T3 is a private SSM state ([Ch 17](./ch-17-mamba-rnn.md)). Classic token compressors (Compressive Transformer, AutoCompressor, ICAE, Gist tokens, LLMLingua) still emit **tokens** you can copy; vendor encrypted blobs you cannot.

**Why it exists.** Attention is finite. Position bias hurts the middle of long prompts. Agent transcripts grow with tool dumps. You either compact, retrieve lazily, or pay and degrade.

**Mechanism — Anthropic `compact_20260112`.** Readable compaction block. Trigger on `input_tokens`, default **150k**, min **50k**. Client **must send the block back**; API drops messages before it. `pause_after_compaction`, custom `instructions`. Sibling levers: `clear_tool_uses_*`, `memory_*`.

**Mechanism — OpenAI.** Server-side `context_management.compact_threshold` or standalone `POST /responses/compact`. Opaque encrypted item (ZDR-friendly). Pass the returned window **as-is**; do not prune. Compact `instructions` should match Responses `instructions`. `truncation: auto` drops messages and **breaks cache** — it is not compacting.

**Harness triggers.** Codex CLI: **90%** of window (can lower, not raise) → OpenAI path encrypted blob, else local handoff summary + ~20k user tokens, then re-read ≤5 recent files. Gemini CLI: default **50%** of a 1M window ≈ 524k; first 70% → XML `state_snapshot`, last 30% verbatim; two LLM passes. opencode: ~96% (leave output headroom). Claude Code: token/event or the model asks `/compact` → readable markdown.

**Boundaries.** Encrypted blob **cannot** move to another vendor — project to T2 *text* first. Compacting ≠ RESET (RESET drops the dialogue body because the *task* changed). Compacting ≠ LLMLingua (portable prompt compression, not a session API).

**Failure modes.** Compacting L0. Pruning OpenAI’s returned window. Treating Gemini’s 50% trigger as “1M is reliable.” Inserting the compact block *before* a cache breakpoint.

## Comparison — vendor APIs + harnesses

| | Anthropic `compact_20260112` | OpenAI `POST /responses/compact` |
| --- | --- | --- |
| Shape | readable compaction block | opaque encrypted item (ZDR-friendly) |
| Trigger | `input_tokens`, default **150k**, min **50k** | client threshold or `context_management.compact_threshold` |
| After | must send the block back; API drops messages before it | pass returned window **as-is**; do not prune |
| Pause | `pause_after_compaction: true` | standalone endpoint *is* an explicit step |
| Custom | `instructions` replaces the default summary prompt | compact `instructions` should match Responses `instructions` |
| Also | `clear_tool_uses_*`, `memory_*` | `truncation: auto` (drops messages, breaks cache) |

| Harness | Trigger | Leaves |
| --- | --- | --- |
| Codex CLI | **90%** of window, can lower not raise | OpenAI path: encrypted blob; else local handoff summary + ~20k user tokens; then re-read ≤5 recent files |
| Gemini CLI | default **50%** (1M window ≈ 524k) | first 70% → XML `state_snapshot`, last 30% verbatim; two LLM passes |
| opencode | ~96% (leave output headroom) | drop after marker; non-LLM tool prune (40k preserve) |
| Claude Code | token / event or model asks `/compact` | readable markdown summary |
| Cursor-style | discover less, fetch lazy, tools write files not inline | keep prefix hot for cache |

**Read (读):**
- [docs — Anthropic Compaction: beta `compact-2026-01-12`; default 150k / min 50k; return the `compaction` block.](https://platform.claude.com/docs/en/build-with-claude/compaction)
- [docs — OpenAI Compaction guide: server-side `compact_threshold` or standalone compact; opaque item; do not prune.](https://developers.openai.com/api/docs/guides/compaction)
- [docs — OpenAI `POST /responses/compact` API reference: the method Codex’s 90% path actually calls.](https://developers.openai.com/api/reference/resources/responses/methods/compact)
- [repo — openai/codex: public Codex CLI harness (90% window compact policy lives in this product).](https://github.com/openai/codex)
- [repo — google-gemini/gemini-cli: public Gemini CLI; default compact near 50% of the 1M window.](https://github.com/google-gemini/gemini-cli)
- [repo — anthropics/claude-code: Claude Code harness; `/compact` → readable markdown.](https://github.com/anthropics/claude-code)
- [repo — anomalyco/opencode: open-source harness with high-watermark compact + tool prune.](https://github.com/anomalyco/opencode)
- [paper — Lost in the Middle (Liu et al. 2023): U-shaped retrieval; the reason compacting exists.](https://arxiv.org/abs/2307.03172)
- [paper — LLMLingua (Jiang et al.): coarse-to-fine prompt compression, up to 20×, still portable tokens.](https://arxiv.org/abs/2310.05736)
- [repo — microsoft/LLMLingua: LLMLingua / LongLLMLingua / LLMLingua-2 implementation.](https://github.com/microsoft/LLMLingua)
- [paper — LongLLMLingua: query-aware long-context compression; ACL 2024.](https://arxiv.org/abs/2310.06839)
- [paper — Gist tokens (Mu et al.): compress prompts into cached gist activations; not a vendor blob.](https://arxiv.org/abs/2304.08467)
- [paper — AutoCompressor (Chevalier et al.): recursive context compression into a summary vector, still tokens/soft prompts.](https://arxiv.org/abs/2305.14788)
- [paper — ICAE (Ge et al.): in-context autoencoder that compresses context into memory slots.](https://arxiv.org/abs/2307.06945)
- [paper — Compressive Transformer (Rae et al.): compress old memories; still tokens.](https://arxiv.org/abs/1911.05507)

[Part III](./README.md)
