# Bibliography

Union of chapter **Read (读)** links. Short notes. Retrieval window: 2026-08. Numbers in chapters still need URL+date in PRs.

## Preface / how this index works

Ch 0: [EN](./00-preface.md) · [中文](../zh/00-preface.md). Concepts in-repo; sources below.

## Value chain / who sits on a layer

**Scale AI** — https://scale.com — Ch 1, 3. B2 labeling / prefs / eval, not “all data.”

**vLLM** — https://github.com/vllm-project/vllm — Ch 1, 2, 6. E1 engine.

**SGLang** — https://github.com/sgl-project/sglang — Ch 1, 6. E1 engine.

**SiliconFlow** — https://www.siliconflow.com — Ch 1, 6. Hosted E2+E5 SKU.

**Fireworks** — https://fireworks.ai — Ch 1, 6. Hosted E2.

**Together AI** — https://www.together.ai — Ch 1, 6. E2+E3.

**FineWeb** — https://huggingface.co/datasets/HuggingFaceFW/fineweb — Ch 2, 3. B1 corpus.

**Common Crawl** — https://commoncrawl.org/ — Ch 2, 3. Raw crawl.

**Phi-1** — https://arxiv.org/abs/2306.11644 — Ch 3. B3 synthetic-data recipe.

**Attention Is All You Need** — https://arxiv.org/abs/1706.03762 — Ch 4. Transformer baseline.

**InstructGPT** — https://arxiv.org/abs/2203.02155 — Ch 4. Public C2 recipe.

**DPO** — https://arxiv.org/abs/2305.18290 — Ch 4.

**DeepSeek-R1** — https://arxiv.org/abs/2501.12948 — Ch 4. Reasoning post-train.

**METR** — https://metr.org — Ch 4. External C3 gate.

**Hugging Face Hub** — https://huggingface.co/models — Ch 4. C4/C5 weights.

**LMSYS Arena** — https://lmarena.ai — Ch 2, 5. Human pref Elo.

**HELM** — https://arxiv.org/abs/2211.09110 — Ch 5.

**Artificial Analysis** — https://artificialanalysis.ai — Ch 5.

**OpenAI API ref** — https://platform.openai.com/docs/api-reference — Ch 6. E5 dialect.

**Anthropic Messages** — https://platform.claude.com/docs/en/api/messages — Ch 6, 19.

**Bedrock Converse** — https://docs.aws.amazon.com/bedrock/latest/userguide/conversation-inference.html — Ch 6.

**WorkOS SSO** — https://workos.com/sso — Ch 7. F1.

**阿里云百炼** — https://www.aliyun.com/product/bailian — Ch 7. China plaza.

**Langfuse** — https://github.com/langfuse/langfuse — Ch 7. F2.

**LangSmith** — https://docs.smith.langchain.com — Ch 7, 13.

**Helicone** — https://github.com/Helicone/helicone · https://docs.helicone.ai — Ch 7, 13, 18.

## Cache / Batch / Flex / relay

**Anthropic prompt caching** — https://platform.claude.com/docs/en/build-with-claude/prompt-caching — Ch 6, 8, 9, 10, 12, 22. Read **0.1×**; write 1.25×/2.0×; max 4 breakpoints.

**OpenAI prompt caching** — https://developers.openai.com/api/docs/guides/prompt-caching — Ch 8, 9, 12, 22. Exact prefix; `prompt_cache_key`.

**Prompt Caching 201** — https://developers.openai.com/cookbook/examples/prompt_caching_201 — Ch 8, 9. Flex vs Batch when cache hits.

**OpenAI Batch** — https://platform.openai.com/docs/guides/batch — Ch 9, 10, 30. Official **50%**, ~24h, JSONL + `custom_id`.

**Anthropic Message Batches** — https://platform.claude.com/docs/en/build-with-claude/batch-processing — Ch 9. Official 50%.

**OpenAI Flex** — https://platform.openai.com/docs/guides/flex-processing — Ch 9, 10. ~50% realtime `service_tier=flex`.

**Anthropic commercial terms** — https://www.anthropic.com/legal/commercial-terms — Ch 10. Resale lives in the contract.

**OpenAI usage policies** — https://openai.com/policies/usage-policies — Ch 10.

## Context / compaction / compressors

**Effective Context Engineering** — Anthropic, 2025-09 — https://www.anthropic.com/engineering/effective-context-engineering-for-ai-agents — Ch 8, 11–13, 16, 22, 30.

**Anthropic compaction** — https://platform.claude.com/docs/en/build-with-claude/compaction — Ch 11–14, 19, 22, 30. Default 150k / min 50k; return the block.

**OpenAI compaction guide** — https://developers.openai.com/api/docs/guides/compaction — Ch 8, 11–14, 16, 19, 22, 30. Opaque item; do not prune.

**OpenAI compact method** — https://developers.openai.com/api/reference/resources/responses/methods/compact — Ch 13, 14.

**Claude cookbook (memory / compact / tool clear)** — https://platform.claude.com/cookbook/tool-use-context-engineering-context-engineering-tools — Ch 8, 11–13, 22.

**LangChain context engineering** — https://docs.langchain.com/oss/python/langchain/context-engineering — Ch 11, 22.

**Lost in the Middle** — Liu et al. 2023 — https://arxiv.org/abs/2307.03172 — Ch 5, 8, 11, 14, 22, 30.

**Needle-in-a-haystack** — https://github.com/gkamradt/LLMTest_NeedleInAHaystack — Ch 5, 22.

**LLMLingua** — https://arxiv.org/abs/2310.05736 · repo https://github.com/microsoft/LLMLingua — Ch 14.

**LongLLMLingua** — https://arxiv.org/abs/2310.06839 — Ch 14, 22.

**Gist tokens** — Mu et al. — https://arxiv.org/abs/2304.08467 — Ch 14, 17.

**AutoCompressor** — Chevalier et al. — https://arxiv.org/abs/2305.14788 — Ch 14, 17.

**ICAE** — Ge et al. — https://arxiv.org/abs/2307.06945 — Ch 14, 17.

**Compressive Transformer** — Rae et al. — https://arxiv.org/abs/1911.05507 — Ch 14, 17.

**MemGPT** — https://arxiv.org/abs/2310.08560 — Ch 11. Paging, not vendor compact.

**Generative Agents** — https://arxiv.org/abs/2304.03442 — Ch 11.

## Relatedness

**TIAGE** — Xie et al., EMNLP 2021 — https://arxiv.org/abs/2109.04562 — Ch 13, 15. Topic-shift; not intent.

**Chinese topic shift** — https://arxiv.org/abs/2305.01195 — Ch 15.

**TextTiling** — Hearst 1997 — https://aclanthology.org/J97-1003/ — Ch 15.

**C99** — Choi — https://aclanthology.org/C00-1070/ — Ch 15.

**Wiki-727K segmentation** — Koshorek et al. — https://arxiv.org/abs/1803.05355 — Ch 15.

**MNLI** — https://arxiv.org/abs/1704.05426 — Ch 15. NLI labels.

## Sequence models

**Mamba** — https://arxiv.org/abs/2312.00752 · https://github.com/state-spaces/mamba — Ch 17.

**Mamba-2** — https://arxiv.org/abs/2405.21060 — Ch 17.

**Waleffe et al. Mamba LMs** — https://arxiv.org/abs/2406.07887 — Ch 17. Phonebook / copy gap.

## Routing / gateways

**RouteLLM** — https://arxiv.org/abs/2406.18665 · https://github.com/lm-sys/RouteLLM · blog https://lmsys.org/blog/2024-07-01-routellm/ — Ch 18.

**FrugalGPT** — https://arxiv.org/abs/2305.05176 — Ch 18.

**Routing and Cascading** — Dekoninck et al. — https://arxiv.org/abs/2410.10347 — Ch 18.

**vLLM Semantic Router** — https://github.com/vllm-project/semantic-router — Ch 18.

**LiteLLM** — https://github.com/BerriAI/litellm · routing docs https://docs.litellm.ai/docs/routing — Ch 1, 7, 10, 18, 19.

**OpenRouter** — https://openrouter.ai/docs · provider routing https://openrouter.ai/docs/guides/routing/provider-selection — Ch 1, 7, 10, 18, 19.

**Portkey gateway** — https://github.com/Portkey-AI/gateway — Ch 7, 18, 19.

**Gemini generateContent** — https://ai.google.dev/api/generate-content — Ch 19.

## NLU

**Joint BERT** — https://arxiv.org/abs/1902.10909 — Ch 20, 21.

**ATIS** — https://aclanthology.org/H90-1021/ — Ch 20.

**Snips** — https://arxiv.org/abs/1805.10190 — Ch 20.

**CLINC150** — https://aclanthology.org/D19-1131/ · https://github.com/clinc/oos-eval — Ch 20.

**BANKING77** — https://arxiv.org/abs/2003.04807 · https://github.com/PolyAI-LDN/task-specific-datasets — Ch 20.

**OpenAI function calling** — https://platform.openai.com/docs/guides/function-calling — Ch 20, 21.

**Anthropic tool use** — https://platform.claude.com/docs/en/agents-and-tools/tool-use/overview — Ch 21.

**Outlines** — https://github.com/dottxt-ai/outlines — Ch 21.

**xgrammar** — https://github.com/mlc-ai/xgrammar — Ch 21.

**Instructor** — https://python.useinstructor.com — Ch 21.

**E5** — https://arxiv.org/abs/2212.03533 — Ch 23.

**GTE** — https://arxiv.org/abs/2308.03281 — Ch 23.

**OpenAI embeddings** — https://platform.openai.com/docs/guides/embeddings — Ch 23.

**bge-m3** — https://huggingface.co/BAAI/bge-m3 — Ch 23.

**tree-sitter** — https://github.com/tree-sitter/tree-sitter — Ch 23.

**Sentence-BERT** — https://arxiv.org/abs/1908.10084 — Ch 23.

## Evals / harnesses

**SWE-bench** — https://arxiv.org/abs/2310.06770 · https://github.com/princeton-nlp/SWE-bench · https://www.swebench.com/ — Ch 2, 3, 5, 25, 26. Verified = 500.

**mini-SWE-agent** — https://github.com/SWE-agent/mini-swe-agent — Ch 26.

**τ-bench** — https://arxiv.org/abs/2406.12045 · https://github.com/sierra-research/tau-bench — Ch 25.

**AgentBench** — https://arxiv.org/abs/2308.03688 · https://github.com/THUDM/AgentBench — Ch 25.

**WebArena** — https://arxiv.org/abs/2307.13854 · https://github.com/web-arena-x/webarena — Ch 25, 28.

**WebVoyager** — https://arxiv.org/abs/2401.13919 — Ch 25, 28.

**OSWorld** — https://arxiv.org/abs/2404.07972 · https://github.com/xlang-ai/OSWorld — Ch 25, 28.

**Codex CLI** — https://github.com/openai/codex — Ch 14, 16, 26, 27. 90% compact path.

**Claude Code** — https://github.com/anthropics/claude-code — Ch 14, 16, 26, 27.

**Gemini CLI** — https://github.com/google-gemini/gemini-cli — Ch 14, 16, 26. Default ~50% of 1M.

**opencode** — https://github.com/anomalyco/opencode — Ch 14, 26.

**OpenHands** — https://github.com/All-Hands-AI/OpenHands — Ch 16, 26.

## Identity / skills / browser / ingress

**MCP Authorization (2025-06-18)** — https://modelcontextprotocol.io/specification/2025-06-18/basic/authorization — Ch 24, 31.

**RFC 8707** — https://www.rfc-editor.org/rfc/rfc8707.html — Ch 24.

**MCP architecture** — https://modelcontextprotocol.io/docs/learn/architecture — Ch 2, 7, 24.

**OAuth 2.1 draft** — https://datatracker.ietf.org/doc/html/draft-ietf-oauth-v2-1-13 — Ch 24.

**Agent Skills** — https://agentskills.io/home · spec https://agentskills.io/specification — Ch 12, 27, 31.

**Cursor skills** — https://cursor.com/docs/context/skills — Ch 27, 31.

**Claude Agent Skills** — https://platform.claude.com/docs/en/agents-and-tools/agent-skills/overview — Ch 27, 31.

**Computer-Using Agent** — https://openai.com/index/computer-using-agent/ — Ch 28, 29. Launch numbers.

**OpenAI computer use guide** — https://platform.openai.com/docs/guides/tools-computer-use — Ch 28.

**CUA sample app** — https://github.com/openai/openai-cua-sample-app — Ch 28.

**Anthropic Computer Use** — https://www.anthropic.com/news/3-5-models-and-computer-use — Ch 28, 29.

**Playwright MCP** — https://github.com/microsoft/playwright-mcp — Ch 28, 29.

**Chrome WebMCP** — https://developer.chrome.com/docs/ai/webmcp/secure-tools · draft https://github.com/webmachinelearning/webmcp — Ch 28, 29.

**ChatGPT export** — https://help.openai.com/en/articles/7260999-how-do-i-export-my-chatgpt-history-and-data — Ch 29, 30.

**Claude data export** — https://support.anthropic.com/en/articles/9451098-how-to-export-your-claude-data — Ch 29.

**GPT Store** — https://help.openai.com/en/articles/8798878-what-is-the-gpt-store — Ch 31.
