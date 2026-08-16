# 第 17 章 · Mamba / RNN compacting

Transformer 注意力对窗口是 O(n²)；KV 随 n 涨。RNN / SSM（状态空间模型）把历史压进固定大小的 `h_t`。**Mamba**（Gu & Dao 2023）是选择性 SSM：不重要的就忘掉。

## 概念

**定义。** Compact 梯子上的 T3——*学出来的*固定大小状态，不是即插即用的 Claude 记忆块。同等规模下，纯 Mamba 在电话簿 / 精确拷贝 ICL 上落后 Transformer（Waleffe et al. 2024）。精确回放一段代码仍然需要：活尾巴、重读文件，或 gist 里的显式摘录。

**为什么存在。** 厂商 T2 API 吐文本或加密 token。研究压缩器吐更短的 token 序列或一个向量 `h_T`。若你想在*同一套架构*里对长 session 做线性扫描，SSM 是候选。它替代不了 Anthropic/OpenAI compact。

**机制。** LSTM/GRU：向量 `h_t`，长程弱。S4/S5：线性 SSM，长卷积视角。Mamba/Mamba-2：选择性 SSM，`d_state` 常 16–128；Mamba-2 把 SSM 连到结构化矩阵。混合（Jamba）：用注意力检索，用 SSM 压缩。要把 T3 当 compacting 用，必须选形态：保住 `h_T`（不可搬运）、映射成 gist tokens（闭源 Claude/GPT 无保证）、或把 `h_T` 解成 markdown gist（可搬运文本，仍有损）。

**边界。** Mamba 权重 ≠ 厂商 compact item。Gist tokens ≠ prompt cache。Compressive Transformer 仍存 token。不要把「我们用了 Mamba」说成「我们实现了 Claude 记忆」。

**失败模式。** 把 Mamba 的 `h_T` 喂给 GPT。指望 128 维状态做电话簿查找。因为「SSM 会记得」就跳过 T0/T1 修剪。

## 对比 — 状态 vs 文本 vs 加密 blob

| 形态 | 你留下什么 | 跨厂商可搬运？ |
| --- | --- | --- |
| A 把 Mamba/`h_T` 当记忆 | 从同一个 SSM 续 | **否**——下游必须是那只 Mamba |
| B `h_T` → gist tokens | Transformer 前缀里的 soft tokens | 闭源 Claude/GPT 上**无保证** |
| C `h_T` → markdown gist | 便宜线性扫描，再 T2 文本 | **可以**当文本；仍有损 |
| Anthropic compact 块 | 可读摘要 | 可当文本（同一厂商更喜欢自己的块） |
| OpenAI 加密 compact item | 不透明 ZDR blob | **否**——换厂商前先投影成文本 |

| 谱系 | 状态 | 当作 compacting |
| --- | --- | --- |
| LSTM / GRU | 向量 `h_t` | 经典细胞；长程弱 |
| S4 / S5 | 线性 SSM | 长卷积视角；语言 < Transformer |
| Mamba / Mamba-2 | 选择性 SSM，`d_state` 常 16–128 | 线性扫描；状态 = 压缩 |
| 混合（Jamba） | Mamba + Attention | 用 attn 检索，用 SSM 压缩 |

**Read (读):**
- [paper — Mamba（Gu & Dao 2023）：选择性 SSM，线性扫描。不是即插即用的 Claude 记忆。](https://arxiv.org/abs/2312.00752)
- [repo — state-spaces/mamba：官方 Mamba 实现。](https://github.com/state-spaces/mamba)
- [paper — Mamba-2（Dao & Gu 2024）：SSM ↔ 结构化矩阵。](https://arxiv.org/abs/2405.21060)
- [paper — Compressive Transformer（Rae et al.）：旧记忆压缩；仍是 token。](https://arxiv.org/abs/1911.05507)
- [paper — Gist tokens（Mu et al.）：提示 → gist 激活；表中的形态 B。](https://arxiv.org/abs/2304.08467)
- [paper — AutoCompressor（Chevalier et al.）：递归压缩；仍属 token/soft-prompt 一族。](https://arxiv.org/abs/2305.14788)
- [paper — ICAE（Ge et al.）：in-context 自编码器 memory slot。](https://arxiv.org/abs/2307.06945)
- [paper — An Empirical Study of Mamba-based Language Models（Waleffe et al.）：相对 Transformer 的电话簿 / 拷贝 ICL 差距。](https://arxiv.org/abs/2406.07887)

[卷三](./README.md)
