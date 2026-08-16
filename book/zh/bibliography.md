# 参考文献

各章 **Read (读)** 链接的并集。短注。检索窗口：2026-08。章节里的数字在 PR 里仍要带 URL+日期。

## 前言 / 这本索引怎么用

第 0 章：[中文](./00-preface.md) · [EN](../en/00-preface.md)。概念写在书里；来源在下面。

## 价值链 / 谁坐在哪一层

**Scale AI** — https://scale.com — 第 1、3 章。B2 标注 / 偏好 / eval，不是「全部数据」。

**vLLM** — https://github.com/vllm-project/vllm — 第 1、2、6 章。E1 引擎。

**SGLang** — https://github.com/sgl-project/sglang — 第 1、6 章。E1 引擎。

**硅基流动 SiliconFlow** — https://www.siliconflow.com — 第 1、6 章。托管 E2+E5 SKU。

**Fireworks** — https://fireworks.ai — 第 1、6 章。托管 E2。

**Together AI** — https://www.together.ai — 第 1、6 章。E2+E3。

**FineWeb** — https://huggingface.co/datasets/HuggingFaceFW/fineweb — 第 2、3 章。B1 语料。

**Common Crawl** — https://commoncrawl.org/ — 第 2、3 章。原始爬取。

**Phi-1** — https://arxiv.org/abs/2306.11644 — 第 3 章。B3 合成数据配方。

**Attention Is All You Need** — https://arxiv.org/abs/1706.03762 — 第 4 章。Transformer 基线。

**InstructGPT** — https://arxiv.org/abs/2203.02155 — 第 4 章。公开 C2 配方。

**DPO** — https://arxiv.org/abs/2305.18290 — 第 4 章。

**DeepSeek-R1** — https://arxiv.org/abs/2501.12948 — 第 4 章。推理后训练。

**METR** — https://metr.org — 第 4 章。外部 C3 门。

**Hugging Face Hub** — https://huggingface.co/models — 第 4 章。C4/C5 权重。

**LMSYS Arena** — https://lmarena.ai — 第 2、5 章。真人偏好 Elo。

**HELM** — https://arxiv.org/abs/2211.09110 — 第 5 章。

**Artificial Analysis** — https://artificialanalysis.ai — 第 5 章。

**OpenAI API 参考** — https://platform.openai.com/docs/api-reference — 第 6 章。E5 方言。

**Anthropic Messages** — https://platform.claude.com/docs/en/api/messages — 第 6、19 章。

**Bedrock Converse** — https://docs.aws.amazon.com/bedrock/latest/userguide/conversation-inference.html — 第 6 章。

**WorkOS SSO** — https://workos.com/sso — 第 7 章。F1。

**阿里云百炼** — https://www.aliyun.com/product/bailian — 第 7 章。中国广场。

**Langfuse** — https://github.com/langfuse/langfuse — 第 7 章。F2。

**LangSmith** — https://docs.smith.langchain.com — 第 7、13 章。

**Helicone** — https://github.com/Helicone/helicone · https://docs.helicone.ai — 第 7、13、18 章。

## Cache / Batch / Flex / 中转

**Anthropic prompt caching** — https://platform.claude.com/docs/en/build-with-claude/prompt-caching — 第 6、8、9、10、12、22 章。Read **0.1×**；write 1.25×/2.0×；最多 4 breakpoint。

**OpenAI prompt caching** — https://developers.openai.com/api/docs/guides/prompt-caching — 第 8、9、12、22 章。精确前缀；`prompt_cache_key`。

**Prompt Caching 201** — https://developers.openai.com/cookbook/examples/prompt_caching_201 — 第 8、9 章。cache 命中时 Flex vs Batch。

**OpenAI Batch** — https://platform.openai.com/docs/guides/batch — 第 9、10、30 章。官方 **50%**，约 24h，JSONL + `custom_id`。

**Anthropic Message Batches** — https://platform.claude.com/docs/en/build-with-claude/batch-processing — 第 9 章。官方 50%。

**OpenAI Flex** — https://platform.openai.com/docs/guides/flex-processing — 第 9、10 章。实时约 50%，`service_tier=flex`。

**Anthropic 商业条款** — https://www.anthropic.com/legal/commercial-terms — 第 10 章。转售写在合同里。

**OpenAI 使用政策** — https://openai.com/policies/usage-policies — 第 10 章。

## 上下文 / compaction / 压缩器

**Effective Context Engineering** — Anthropic，2025-09 — https://www.anthropic.com/engineering/effective-context-engineering-for-ai-agents — 第 8、11–13、16、22、30 章。

**Anthropic compaction** — https://platform.claude.com/docs/en/build-with-claude/compaction — 第 11–14、19、22、30 章。默认 150k / 最低 50k；要把块带回去。

**OpenAI compaction 指南** — https://developers.openai.com/api/docs/guides/compaction — 第 8、11–14、16、19、22、30 章。不透明 item；不要剪。

**OpenAI compact 方法** — https://developers.openai.com/api/reference/resources/responses/methods/compact — 第 13、14 章。

**Claude cookbook（memory / compact / 清 tool）** — https://platform.claude.com/cookbook/tool-use-context-engineering-context-engineering-tools — 第 8、11–13、22 章。

**LangChain 上下文工程** — https://docs.langchain.com/oss/python/langchain/context-engineering — 第 11、22 章。

**Lost in the Middle** — Liu et al. 2023 — https://arxiv.org/abs/2307.03172 — 第 5、8、11、14、22、30 章。

**Needle-in-a-haystack** — https://github.com/gkamradt/LLMTest_NeedleInAHaystack — 第 5、22 章。

**LLMLingua** — https://arxiv.org/abs/2310.05736 · 仓库 https://github.com/microsoft/LLMLingua — 第 14 章。

**LongLLMLingua** — https://arxiv.org/abs/2310.06839 — 第 14、22 章。

**Gist tokens** — Mu et al. — https://arxiv.org/abs/2304.08467 — 第 14、17 章。

**AutoCompressor** — Chevalier et al. — https://arxiv.org/abs/2305.14788 — 第 14、17 章。

**ICAE** — Ge et al. — https://arxiv.org/abs/2307.06945 — 第 14、17 章。

**Compressive Transformer** — Rae et al. — https://arxiv.org/abs/1911.05507 — 第 14、17 章。

**MemGPT** — https://arxiv.org/abs/2310.08560 — 第 11 章。分页，不是厂商 compact。

**Generative Agents** — https://arxiv.org/abs/2304.03442 — 第 11 章。

## 相关

**TIAGE** — Xie et al.，EMNLP 2021 — https://arxiv.org/abs/2109.04562 — 第 13、15 章。topic-shift；不是意图。

**中文对话 Topic Shift** — https://arxiv.org/abs/2305.01195 — 第 15 章。

**TextTiling** — Hearst 1997 — https://aclanthology.org/J97-1003/ — 第 15 章。

**C99** — Choi — https://aclanthology.org/C00-1070/ — 第 15 章。

**Wiki-727K 切分** — Koshorek et al. — https://arxiv.org/abs/1803.05355 — 第 15 章。

**MNLI** — https://arxiv.org/abs/1704.05426 — 第 15 章。NLI 标签。

## 序列模型

**Mamba** — https://arxiv.org/abs/2312.00752 · https://github.com/state-spaces/mamba — 第 17 章。

**Mamba-2** — https://arxiv.org/abs/2405.21060 — 第 17 章。

**Waleffe et al. Mamba LMs** — https://arxiv.org/abs/2406.07887 — 第 17 章。电话簿 / 拷贝差距。

## 路由 / 网关

**RouteLLM** — https://arxiv.org/abs/2406.18665 · https://github.com/lm-sys/RouteLLM · 博客 https://lmsys.org/blog/2024-07-01-routellm/ — 第 18 章。

**FrugalGPT** — https://arxiv.org/abs/2305.05176 — 第 18 章。

**Routing and Cascading** — Dekoninck et al. — https://arxiv.org/abs/2410.10347 — 第 18 章。

**vLLM Semantic Router** — https://github.com/vllm-project/semantic-router — 第 18 章。

**LiteLLM** — https://github.com/BerriAI/litellm · 路由文档 https://docs.litellm.ai/docs/routing — 第 1、7、10、18、19 章。

**OpenRouter** — https://openrouter.ai/docs · provider routing https://openrouter.ai/docs/guides/routing/provider-selection — 第 1、7、10、18、19 章。

**Portkey gateway** — https://github.com/Portkey-AI/gateway — 第 7、18、19 章。

**Gemini generateContent** — https://ai.google.dev/api/generate-content — 第 19 章。

## NLU

**Joint BERT** — https://arxiv.org/abs/1902.10909 — 第 20、21 章。

**ATIS** — https://aclanthology.org/H90-1021/ — 第 20 章。

**Snips** — https://arxiv.org/abs/1805.10190 — 第 20 章。

**CLINC150** — https://aclanthology.org/D19-1131/ · https://github.com/clinc/oos-eval — 第 20 章。

**BANKING77** — https://arxiv.org/abs/2003.04807 · https://github.com/PolyAI-LDN/task-specific-datasets — 第 20 章。

**OpenAI function calling** — https://platform.openai.com/docs/guides/function-calling — 第 20、21 章。

**Anthropic tool use** — https://platform.claude.com/docs/en/agents-and-tools/tool-use/overview — 第 21 章。

**Outlines** — https://github.com/dottxt-ai/outlines — 第 21 章。

**xgrammar** — https://github.com/mlc-ai/xgrammar — 第 21 章。

**Instructor** — https://python.useinstructor.com — 第 21 章。

**E5** — https://arxiv.org/abs/2212.03533 — 第 23 章。

**GTE** — https://arxiv.org/abs/2308.03281 — 第 23 章。

**OpenAI embeddings** — https://platform.openai.com/docs/guides/embeddings — 第 23 章。

**bge-m3** — https://huggingface.co/BAAI/bge-m3 — 第 23 章。

**tree-sitter** — https://github.com/tree-sitter/tree-sitter — 第 23 章。

**Sentence-BERT** — https://arxiv.org/abs/1908.10084 — 第 23 章。

## Evals / harnesses

**SWE-bench** — https://arxiv.org/abs/2310.06770 · https://github.com/princeton-nlp/SWE-bench · https://www.swebench.com/ — 第 2、3、5、25、26 章。Verified = 500。

**mini-SWE-agent** — https://github.com/SWE-agent/mini-swe-agent — 第 26 章。

**τ-bench** — https://arxiv.org/abs/2406.12045 · https://github.com/sierra-research/tau-bench — 第 25 章。

**AgentBench** — https://arxiv.org/abs/2308.03688 · https://github.com/THUDM/AgentBench — 第 25 章。

**WebArena** — https://arxiv.org/abs/2307.13854 · https://github.com/web-arena-x/webarena — 第 25、28 章。

**WebVoyager** — https://arxiv.org/abs/2401.13919 — 第 25、28 章。

**OSWorld** — https://arxiv.org/abs/2404.07972 · https://github.com/xlang-ai/OSWorld — 第 25、28 章。

**Codex CLI** — https://github.com/openai/codex — 第 14、16、26、27 章。90% compact 路径。

**Claude Code** — https://github.com/anthropics/claude-code — 第 14、16、26、27 章。

**Gemini CLI** — https://github.com/google-gemini/gemini-cli — 第 14、16、26 章。默认约 1M 的 50%。

**opencode** — https://github.com/anomalyco/opencode — 第 14、26 章。

**OpenHands** — https://github.com/All-Hands-AI/OpenHands — 第 16、26 章。

## 身份 / skills / 浏览器 / 入口

**MCP Authorization（2025-06-18）** — https://modelcontextprotocol.io/specification/2025-06-18/basic/authorization — 第 24、31 章。

**RFC 8707** — https://www.rfc-editor.org/rfc/rfc8707.html — 第 24 章。

**MCP 架构** — https://modelcontextprotocol.io/docs/learn/architecture — 第 2、7、24 章。

**OAuth 2.1 draft** — https://datatracker.ietf.org/doc/html/draft-ietf-oauth-v2-1-13 — 第 24 章。

**Agent Skills** — https://agentskills.io/home · 规范 https://agentskills.io/specification — 第 12、27、31 章。

**Cursor skills** — https://cursor.com/docs/context/skills — 第 27、31 章。

**Claude Agent Skills** — https://platform.claude.com/docs/en/agents-and-tools/agent-skills/overview — 第 27、31 章。

**Computer-Using Agent** — https://openai.com/index/computer-using-agent/ — 第 28、29 章。发布数字。

**OpenAI computer use 指南** — https://platform.openai.com/docs/guides/tools-computer-use — 第 28 章。

**CUA sample app** — https://github.com/openai/openai-cua-sample-app — 第 28 章。

**Anthropic Computer Use** — https://www.anthropic.com/news/3-5-models-and-computer-use — 第 28、29 章。

**Playwright MCP** — https://github.com/microsoft/playwright-mcp — 第 28、29 章。

**Chrome WebMCP** — https://developer.chrome.com/docs/ai/webmcp/secure-tools · 草案 https://github.com/webmachinelearning/webmcp — 第 28、29 章。

**ChatGPT 导出** — https://help.openai.com/en/articles/7260999-how-do-i-export-my-chatgpt-history-and-data — 第 29、30 章。

**Claude 数据导出** — https://support.anthropic.com/en/articles/9451098-how-to-export-your-claude-data — 第 29 章。

**GPT Store** — https://help.openai.com/en/articles/8798878-what-is-the-gpt-store — 第 31 章。
