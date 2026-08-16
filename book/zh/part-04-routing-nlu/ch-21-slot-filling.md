# 第 21 章 · Slot filling

意图之后填参数。经典：token BIO 标注。现在：JSON Schema 结构化解码。

## 概念

**定义。** 槽是意图的参数：目的地城市、文件路径、`ref`。经典 ATIS 用逐 token 的 BIO + CRF。生产 agent 用工具 JSON：`name` + 按 schema 校验的 `arguments`。

**为什么存在。** 没有槽的意图就是「订机票」却没有机场。缺槽策略是对话管理：追问、默认、从 session 继承。继承会打到上下文层：RESET 必须**切断**槽继承；CONTINUE 才可以解析「那个文件」。

**机制。** token 上的 BIO/BIOES。嵌套槽用 span 抽取。Joint BERT：意图头 + 槽位头。Function calling：OpenAI tools、Anthropic `tool_use`、Gemini `function_call`。约束解码：Outlines、xgrammar、llama.cpp grammars。Instructor / Pydantic：类型即 schema，重试或追问。

**边界。** 槽不是相关。L0 里 schema 膨胀会打破 prompt cache。工具返回 5 万行日志是 compacting 问题，不是槽问题。

**失败模式。** 用户已经改做 CSS，还继承 `repo=auth-service`。无约束解码吐出非法 JSON 然后永远重试。

## 对比

| 方法 | 输出 | 背景 |
| --- | --- | --- |
| BIO / BIOES | 逐 token 槽标签 | ATIS 槽；CRF |
| Span 抽取 | 起止 + 类型 | 嵌套槽 |
| 联合模型 | 意图头 + 槽位头 | Joint BERT（2019） |
| Function calling | `name` + arguments JSON | OpenAI tools、Anthropic `tool_use`、Gemini `function_call` |
| 约束解码 | JSON Schema / CFG | Outlines、xgrammar、llama.cpp grammars |
| Instructor / Pydantic | 类型即 schema | 应用层校验；重试或追问 |

**Read (读):**
- [paper — Joint BERT（Chen et al. 2019）：联合意图+槽基线。](https://arxiv.org/abs/1902.10909)
- [docs — OpenAI function calling：生产 agent 里槽是 JSON 参数，不是 BIO。](https://platform.openai.com/docs/guides/function-calling)
- [docs — Anthropic tool use：`tool_use` / `tool_result` 作为槽通道。](https://platform.claude.com/docs/en/agents-and-tools/tool-use/overview)
- [repo — dottxt-ai/outlines：按 JSON Schema / CFG 约束生成。](https://github.com/dottxt-ai/outlines)
- [repo — mlc-ai/xgrammar：serving 里用的快速结构化解码。](https://github.com/mlc-ai/xgrammar)
- [docs — Instructor：Pydantic 类型即模型必须填的 schema。](https://python.useinstructor.com)

[卷四](./README.md)
