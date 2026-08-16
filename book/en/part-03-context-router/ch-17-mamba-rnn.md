# Ch 17 · Mamba / RNN compacting

Transformer attention is O(n²) in the window; KV grows with n. RNN / SSM（状态空间模型） crushes history into a fixed-size `h_t`. **Mamba** (Gu & Dao 2023) is a selective SSM: forget the unimportant.

## Concept

**Definition.** T3 in the compact ladder — a *learned* fixed-size state, not a drop-in Claude memory blob. Pure Mamba lags same-size Transformers on phonebook / exact-copy ICL (Waleffe et al. 2024). Exact replay of a code span still needs: live tail, file re-read, or an explicit excerpt in the gist.

**Why it exists.** Vendor T2 APIs emit text or encrypted tokens. Research compressors emit shorter token sequences or a vector `h_T`. If you want linear scan over a long session *inside one architecture*, SSM is the candidate. It does not replace Anthropic/OpenAI compact.

**Mechanism.** LSTM/GRU: vector `h_t`, weak long range. S4/S5: linear SSM, long-conv view. Mamba/Mamba-2: selective SSM, `d_state` often 16–128; Mamba-2 connects SSM to structured matrices. Hybrids (Jamba): retrieve via attention, compress via SSM. To *use* T3 as compacting you must pick a form: keep `h_T` (not portable), map to gist tokens (no guarantee on closed Claude/GPT), or decode `h_T` → markdown gist (portable text, still lossy).

**Boundaries.** Mamba weights ≠ vendor compact item. Gist tokens ≠ prompt cache. Compressive Transformer still stores tokens. Do not advertise “we use Mamba” as “we implemented Claude memory.”

**Failure modes.** Feeding a Mamba `h_T` to GPT. Expecting phonebook lookup from a 128-dim state. Skipping T0/T1 prune because “SSM will remember.”

## Comparison — state vs text vs encrypted blob

| Form | What you keep | Portable across vendors? |
| --- | --- | --- |
| A Mamba/`h_T` as memory | continue from same SSM | **No** — downstream must be that Mamba |
| B `h_T` → gist tokens | soft tokens in a Transformer prefix | **No guarantee** on closed Claude/GPT |
| C `h_T` → markdown gist | cheap linear scan, then T2 text | **Yes** as text; still lossy |
| Anthropic compact block | readable summary | Yes as text (same vendor prefers its block) |
| OpenAI encrypted compact item | opaque ZDR blob | **No** — project to text before swap |

| Lineage | State | As compacting |
| --- | --- | --- |
| LSTM / GRU | vector `h_t` | classic cell; weak long range |
| S4 / S5 | linear SSM | long conv view; language < Transformer |
| Mamba / Mamba-2 | selective SSM, `d_state` often 16–128 | linear scan; state = compression |
| Hybrid (Jamba) | Mamba + Attention | retrieve via attn, compress via SSM |

**Read (读):**
- [paper — Mamba (Gu & Dao 2023): selective SSM, linear-time scan. Not a drop-in Claude memory.](https://arxiv.org/abs/2312.00752)
- [repo — state-spaces/mamba: official Mamba implementation.](https://github.com/state-spaces/mamba)
- [paper — Mamba-2 (Dao & Gu 2024): SSM ↔ structured matrices.](https://arxiv.org/abs/2405.21060)
- [paper — Compressive Transformer (Rae et al.): old memories compressed; still tokens.](https://arxiv.org/abs/1911.05507)
- [paper — Gist tokens (Mu et al.): prompt → gist activations; form B in the table.](https://arxiv.org/abs/2304.08467)
- [paper — AutoCompressor (Chevalier et al.): recursive compression; still in the token/soft-prompt family.](https://arxiv.org/abs/2305.14788)
- [paper — ICAE (Ge et al.): in-context autoencoder memory slots.](https://arxiv.org/abs/2307.06945)
- [paper — An Empirical Study of Mamba-based Language Models (Waleffe et al.): phonebook / copy ICL gap vs Transformers.](https://arxiv.org/abs/2406.07887)

[Part III](./README.md)
