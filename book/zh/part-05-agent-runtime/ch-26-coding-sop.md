# 第 26 章 · Coding agent SOP

SOP = 可重复的步骤序列，不是权重。在 coding agent 里它是 harness **外循环**。

## 概念

**定义。** 定位 → 计划 → 打补丁 → 收尾。SWE-bench 的隐藏 SOP：某个 commit 上的仓库 + issue 文本 + 步数预算 → unified diff，用测试打分。不同脚手架（几乎只有 bash vs 编辑工具）能把分数挪几分到 >10 分——是 SOP，不是智商。SOP 一改就要改版本；它们会改 eval。

**为什么存在。** 模型不是产品。搜索、编辑、跑测试、停下的那个循环才是。Skills vs 规则 vs 主循环：[第 27 章](./ch-27-skills-auto-sop.md)。公开 harness：Codex CLI、Claude Code、Gemini CLI、opencode、OpenHands。

**机制。** 载体：system-prompt 步骤、钩子（工具前后）、规则文件、Skills。mini-SWE-agent 表明工具面可以很小仍能得分。Compact 之后重读最近文件（Codex ≤5）是 SOP 的一部分，不是另一个「记忆产品」。

**边界。** SOP 不是后训练。改 SOP、模型不变，就是新系统。不要把每份 SOP 都倒进 system（L0 膨胀）。

**失败模式。** eval 两次之间 prompt 无版本地漂。无限修复循环没有停止规则。把 OpenHands 和 mini-SWE-agent 的分数当成同一个 SWE-bench。

## 对比

| 阶段 | 常见步骤 |
| --- | --- |
| 定位 | 读 issue → 搜符号 → 打开文件 → 写下任务边界 |
| 计划 | 文件级计划；有的产品从 plan 模式开始 |
| 打补丁 | 编辑 → linter → 测试 → 再读失败 |
| 收尾 | diff 自检 → commit message → 停 |

**Read (读):**
- [paper — SWE-bench：oracle 是测试；隐藏变量是脚手架。](https://arxiv.org/abs/2310.06770)
- [repo — princeton-nlp/SWE-bench。](https://github.com/princeton-nlp/SWE-bench)
- [repo — SWE-agent / mini-SWE-agent（SWE-bench 团队）：工具面可以很小仍能得分。](https://github.com/SWE-agent/mini-swe-agent)
- [repo — openai/codex：Codex CLI 外循环。](https://github.com/openai/codex)
- [repo — anthropics/claude-code：Claude Code 外循环。](https://github.com/anthropics/claude-code)
- [repo — google-gemini/gemini-cli。](https://github.com/google-gemini/gemini-cli)
- [repo — anomalyco/opencode。](https://github.com/anomalyco/opencode)
- [repo — All-Hands-AI/OpenHands：开源 coding agent runtime。](https://github.com/All-Hands-AI/OpenHands)

[卷五](./README.md)
