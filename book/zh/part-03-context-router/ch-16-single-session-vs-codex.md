# 第 16 章 · 单 session vs Codex 线程

用户看见的对象：一条连续聊天 vs 许多任务线程。压缩策略跟着对象走。

## 概念

**定义。** **单 session** 产品展示一条连续聊天。切题检测必须够好，否则鉴权约束会漏进 CSS 任务。好处：用户不用管理线程；L0（tools/system）可以整晚保持热。**Codex 式线程**按任务切（桌面端常常按项目）。Compact 之后 Codex 最多重读 5 个最近编辑的文件——文件系统当外部记忆；和懒取是同一个想法，harness 不同。

**为什么存在。** UX 选定对象；compacting 在下游。一条聊天的 UX 里 FORK 必须显式。Codex 只要再开一条线程。

**机制。** 单 session：在**同一条线**里 compact 或少塞；切题时在线 RESET 或 COMPACT。Codex：窗口 **90%** 时经 Responses compact 自动压；OpenAI 路径加密；本地路径手写 handoff；线程常常粘在一颗模型上（换模型 ≈ 新上下文）；多线程是一等公民。

**边界。** Session 对象 ≠ 相关模型 ≠ 厂商 compact API。Cursor 类懒取文件不是 Codex 的 90% blob。同一聊天里换模型会杀掉 cache 和加密 item；Codex 常常靠把线程粘在模型上躲开这一点。

**例子。** 用户说「现在做 CSS」。单 session 必须 RESET_PREFIX，否则旧鉴权笔记会污染。Codex 用户另开线程；旧的冻住。

**失败模式。** 真切题了却 compact 而不 RESET。把 OpenAI 加密 compact item 搬进 Claude 线程。单 session 在 RESET 之后仍继承 slot。

## 对比

| 维度 | 单 session 产品 | Codex |
| --- | --- | --- |
| 用户对象 | 一条连续聊天 | 按任务的线程；桌面端常按项目 |
| 满窗 | 在**同一条线**里 compact 或少塞 | 窗口 **90%** 经 Responses compact 自动压 |
| 摘要形态 | 可读或自管 | OpenAI 路径加密；本地路径手写 handoff |
| 换模型 | 用户/路由在同一条线里 | 线程常粘一颗模型；换 ≈ 新上下文 |
| 并行 | 子 agent 在旁；主聊天仍是一条 | 多线程是一等公民 |
| 切题 | 必须在线 RESET 或 COMPACT，否则旧约束污染 | 另开线程；旧的冻住 |

**Read (读):**
- [docs — OpenAI compaction：Codex 90% 路径返回什么（加密 item；不要剪）。](https://developers.openai.com/api/docs/guides/compaction)
- [repo — openai/codex：公开 CLI，session 对象是线程 + 90% compact。](https://github.com/openai/codex)
- [docs — Anthropic 有效上下文工程：Claude Code compact + 摘要后重读最近访问的文件。](https://www.anthropic.com/engineering/effective-context-engineering-for-ai-agents)
- [repo — anthropics/claude-code：单线 session harness，带 `/compact`。](https://github.com/anthropics/claude-code)
- [repo — google-gemini/gemini-cli：1M 窗上 50% compact——另一种 session 对象、另一个阈值。](https://github.com/google-gemini/gemini-cli)
- [repo — All-Hands-AI/OpenHands：开源 agent runtime，有自己的 session/线程故事。](https://github.com/All-Hands-AI/OpenHands)

[卷三](./README.md)
