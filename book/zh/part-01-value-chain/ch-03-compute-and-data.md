# 第 3 章 · 算力与数据

成本地板（芯片 + GPU 云），以及 9 步草图塞进「Scale AI」四个字里的数据供应链。

## 概念

**定义。** A1 芯片和 A2 GPU 云是物理地板：没有卡，就没有训练，也没有第 4/5 步。B1–B4 才是*数据*链：原始爬取、人工标注/偏好、合成/蒸馏、评测集。Scale AI 是 **B2**——最贵、最好报价的那一刀——不是「数据」本身。

**为什么存在。** 产品新闻只谈模型。账单更早开始：谁占着 H100/B200 时间，谁买了爬取授权，谁付了标注员，你将被打分的评测集是谁发的。

**机制。** 预训练读 B1（Common Crawl、FineWeb、授权新闻、GitHub）。后训练读 B2 偏好和 B3 教师轨迹。出荷/采购门读 B4（SWE-bench、MMLU、Arena）。人工 B2 按小时 / 条 / 偏好对计费。合成 B3 按教师 token + 学生 GPU-hour 计费。评测集常常是免费产物；其上的人工评测才收费。

**边界。** B4 评测集既是数据产品，也是 [第 5 章](./ch-05-eval-gate.md) 的输入。它不是标注公司。应用公司通常从不碰 A1；他们买 [第 6 章](./ch-06-inference-api.md) 的 token。蒸馏（B3）不是「我们训了一个新基座」。

**例子。** FineWeb 是可以点名的、过滤过的 Common Crawl 衍生品。SWE-bench 是以单元测试为 oracle 的评测集——你不是向 Scale 付费才「拥有 SWE-bench」；你付费的是额外人工审或私有切片。

**失败模式。** 把所有数据工作都叫「RLHF」。把教师偏见拷进学生（B3）还当 ground truth。把 GPU-hour 报价当成 token 价（A2 vs E2）。

## 对比 — Scale 标注 vs 合成 / 蒸馏

| | B2 人工标注 / 偏好 | B3 合成 / 蒸馏 |
| --- | --- | --- |
| 谁 | Scale, Surge, Mercor, Labelbox | 实验室配方；教师 GPT/Claude/DeepSeek |
| 账单 | 小时 / 条 / 偏好对 | 教师 token + 学生 GPU-hour |
| 信号 | RLHF 偏好、专家轨迹 | 教师 SFT、self-play 轨迹 |
| 失败 | 慢、贵、标注漂移 | 教师偏见拷进学生 |

| 层 | 卖方 | 账单 |
| --- | --- | --- |
| A1 芯片 | NVIDIA, TPU, 昇腾, Groq LPU, Cerebras | 卡 / 整机；常折进 token 价 |
| A2 GPU 云 | AWS, CoreWeave, 火山引擎, 阿里云 | GPU-hour |
| B1 原始 | Common Crawl, FineWeb, GitHub, 授权新闻 | 爬取 + 授权 |
| B4 评测 | SWE-bench, MMLU, LM Arena, Scale SEAL | 集合常免费；人工评测收费 |

**Read (读):**
- [docs — Scale AI：B2 标注 / 偏好 / eval 卖方，不是爬取本身。](https://scale.com)
- [docs — FineWeb 数据集卡：Hugging Face 公开过滤网页语料（B1）。](https://huggingface.co/datasets/HuggingFaceFW/fineweb)
- [docs — Common Crawl：FineWeb 和多数预训练配比仍依赖的原始爬取。](https://commoncrawl.org/)
- [docs — SWE-bench：编程*评测集*（B4），不是标注公司。](https://www.swebench.com/)
- [repo — princeton-nlp/SWE-bench：原始 issue、测试与 harness 代码。](https://github.com/princeton-nlp/SWE-bench)
- [paper — Phi-1（Gunasekar et al.）：教科书级合成数据作为 B3 配方，不是新芯片。](https://arxiv.org/abs/2306.11644)
- [docs — NVIDIA 数据中心 GPU 产品页：A1 是目录，不是 token 表。](https://www.nvidia.com/en-us/data-center/products/)

[卷一](./README.md)
