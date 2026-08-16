# 第 25 章 · Agent evals

分数 = 模型 + harness + 工具 + 提示 + 步数预算。公开榜很窄。

## 概念

**定义。** Agent eval 量的是*系统*，不是 checkpoint。SWE-bench：issue → patch → 测试。τ-bench：多轮工具 + 数据库终态。AgentBench：OS / DB / web / 谜题。WebArena / WebVoyager / OSWorld：GUI。上下文层需要**自己的切片**，不能只看 SWE-bench：（1）compact 之后还能续；（2）换题之后旧约束不泄漏；（3）外来 ChatGPT ingest 再接着干。

**为什么存在。** Harness 差能把分数挪几分到 >10 分。买方若比较「Claude vs GPT 在 SWE-bench」却不点名脚手架，比的是两套不同系统。

**机制。** 冻结模型、harness、工具、提示、步数预算。跑。按榜定义的指标报。离线该走 Batch（[第 9 章](../part-02-inference-cost/ch-09-batch-flex.md)）。CUA 发布数字（OpenAI，公开快照）：WebVoyager 87%，WebArena 58.1%，OSWorld 38.1%（人类 OSWorld 72.4%）。快照，不是法律。

**边界。** 公开编程榜 ≠ 守策略的 τ-bench ≠ GUI OSWorld。内部评测：你的 SOP、仓库、ACL——断言 + LLM-as-judge + 审计。

**失败模式。** 刷榜。换了编辑工具却宣称新模型。用交互 1.0× 价跑 1 万次过夜 eval。

## 对比

| 榜 | 量什么 | 规模 | 分数 |
| --- | --- | --- | --- |
| SWE-bench Verified | issue → patch 测试 | 500 | fail-to-pass ∧ keep-pass |
| τ-bench / τ²-bench | 多轮工具 + 数据库终态 | retail/airline 等 | 策略 + 状态 |
| AgentBench | OS / DB / web / 谜题 | 8 个环境 | 分环境成功率 |
| WebArena / WebVoyager / OSWorld | 浏览器 / 桌面 GUI | 任务集 | 轨迹成功 |
| 内部 | 你的 SOP、仓库、ACL | 你的 | 断言 + LLM-as-judge + 审计 |

**Read (读):**
- [paper — SWE-bench（Jimenez et al.）。](https://arxiv.org/abs/2310.06770)
- [repo — princeton-nlp/SWE-bench。](https://github.com/princeton-nlp/SWE-bench)
- [docs — swebench.com（Verified = 500）。](https://www.swebench.com/)
- [paper — τ-bench（Yao / Sierra）：工具 + 数据库终态，不只 patch。](https://arxiv.org/abs/2406.12045)
- [repo — sierra-research/tau-bench。](https://github.com/sierra-research/tau-bench)
- [paper — AgentBench（Liu et al.）：8 个环境，分环境成功。](https://arxiv.org/abs/2308.03688)
- [repo — THUDM/AgentBench。](https://github.com/THUDM/AgentBench)
- [paper — WebArena（Zhou et al.）：真实感网页任务。](https://arxiv.org/abs/2307.13854)
- [repo — web-arena-x/webarena。](https://github.com/web-arena-x/webarena)
- [paper — WebVoyager（He et al.）：带视觉的端到端网页 agent。](https://arxiv.org/abs/2401.13919)
- [paper — OSWorld（Xie et al.）：桌面 GUI agent；人类 72.4% 作参照。](https://arxiv.org/abs/2404.07972)
- [repo — xlang-ai/OSWorld。](https://github.com/xlang-ai/OSWorld)

[卷五](./README.md)
