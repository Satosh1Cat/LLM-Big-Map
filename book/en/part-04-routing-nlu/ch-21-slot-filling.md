# Ch 21 · Slot filling（槽填充）

After intent, fill arguments. Classic: token BIO tagging. Now: JSON Schema structured decode.

## Concept

**Definition.** Slots are the arguments of an intent: destination city, file path, `ref`. Classic ATIS used per-token BIO tags + CRF. Production agents use tool JSON: `name` + `arguments` validated against a schema.

**Why it exists.** Intent without slots is “book a flight” with no airport. Missing-slot policy is dialogue management: clarify, default, inherit from session. Inheritance hits the context layer: RESET must **cut** slot inherit; CONTINUE may resolve “that file”.

**Mechanism.** BIO/BIOES on tokens. Span extract for nested slots. Joint BERT: intent head + slot head. Function calling: OpenAI tools, Anthropic `tool_use`, Gemini `function_call`. Constrained decode: Outlines, xgrammar, llama.cpp grammars. Instructor / Pydantic: types as schema, retry or ask.

**Boundaries.** Slots are not relatedness. Schema bloat in L0 breaks prompt cache. A tool that returns 50k of logs is a compacting problem, not a slot problem.

**Failure modes.** Inheriting `repo=auth-service` after the user switched to CSS. Letting unconstrained decode emit invalid JSON and retrying forever.

## Comparison

| Method | Output | Background |
| --- | --- | --- |
| BIO / BIOES | per-token slot tag | ATIS slots; CRF |
| Span extract | start/end + type | nested slots |
| Joint model | intent head + slot head | Joint BERT (2019) |
| Function calling | `name` + arguments JSON | OpenAI tools, Anthropic `tool_use`, Gemini `function_call` |
| Constrained decode | JSON Schema / CFG | Outlines, xgrammar, llama.cpp grammars |
| Instructor / Pydantic | types as schema | app-level validate; retry or ask |

**Read (读):**
- [paper — Joint BERT (Chen et al. 2019): the joint intent+slot baseline.](https://arxiv.org/abs/1902.10909)
- [docs — OpenAI function calling: slots as JSON arguments, not BIO, in production agents.](https://platform.openai.com/docs/guides/function-calling)
- [docs — Anthropic tool use: `tool_use` / `tool_result` as the slot channel.](https://platform.claude.com/docs/en/agents-and-tools/tool-use/overview)
- [repo — dottxt-ai/outlines: constrained generation against JSON Schema / CFG.](https://github.com/dottxt-ai/outlines)
- [repo — mlc-ai/xgrammar: fast structured decoding used in serving stacks.](https://github.com/mlc-ai/xgrammar)
- [docs — Instructor: Pydantic types as the schema the model must fill.](https://python.useinstructor.com)

[Part IV](./README.md)
