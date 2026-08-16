# 第 23 章 · 特征提取

把原始请求变成向量 / 标量，**给别的判定器用**。特征不决策。一个向量可以喂给相关、意图和模型路由——**列分开**。

## 概念

**定义。** 特征是测量：长度、语言、围栏比例、路径、查询 embed、与 gist 的余弦、距上次 compact 的轮数、cache hit、TPM。它们是输入。相关、意图、模型路由各自有自己的头。

**为什么存在。** 热路径等不起第二个 LLM。预算是毫秒到一次 embed。共享*列*可以；融合*决策*就会把片段揉在一起。

**机制。** 表面规则。符号抽取（regex / tree-sitter）。句向量（E5、GTE、text-embedding-3、bge-m3）。交叉：query–gist 余弦、token Jaccard。Session 计数。Gateway 指标。结构（未闭合的 `tool_call`、plan 模式）。

**边界。** Embedding 不是路由器。余弦不是 SHIFT。第二个 LLM 分类器属于模型路由 / 意图服务，除非你愿意把它买在上下文层热路径上。

**失败模式。** 一个 softmax 盖住「切题 + 意图 + 便宜-vs-强」。每次请求上 8k 维 embed 再奇怪 p99。

## 对比

| 族 | 例子 | 典型算法 |
| --- | --- | --- |
| 表面 | 长度、语言、围栏比例、`?`、命令动词 | 规则 |
| 符号 | 路径、仓库、issue id、import、函数名 | regex / tree-sitter |
| 句向量 | 查询 embed、尾巴均值、gist embed | E5, GTE, text-embedding-3, bge-m3 |
| 交叉 | query–gist 余弦、token Jaccard | 点积 |
| Session | 轮数、距 compact、距换模型、空闲 | 计数器 |
| 运行时 | cache hit、剩余窗口、TPM、plan、是否有工具 | Gateway 指标 |
| 结构 | 未闭合 `tool_call`、plan 模式 | 状态机 |

**Read (读):**
- [paper — E5（Wang et al.）：弱监督文本 embedding，仍常当句向量。](https://arxiv.org/abs/2212.03533)
- [paper — GTE（Li et al.）：另一族广泛托管的 embedding。](https://arxiv.org/abs/2308.03281)
- [docs — OpenAI embeddings：`text-embedding-3-*` 作为托管句向量。](https://platform.openai.com/docs/guides/embeddings)
- [docs — BAAI bge-m3：多语 embed，常用于 query–gist 余弦。](https://huggingface.co/BAAI/bge-m3)
- [repo — tree-sitter：许多「符号」特征背后的解析器（函数名，不是语义）。](https://github.com/tree-sitter/tree-sitter)
- [paper — Sentence-BERT（Reimers & Gurevych）：相关判定仍在借用的句向量想法。](https://arxiv.org/abs/1908.10084)

[卷四](./README.md)
