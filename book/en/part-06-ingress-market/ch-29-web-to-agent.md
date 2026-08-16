# Ch 29 · Web-to-Agent

Turn work that already happened in the browser into agent input. Three **physical** paths — do not mash.

## Concept

**Definition.** Path 1: page context — an extension reads DOM / selection / URL of *this* page. Path 2: session handoff — ChatGPT/Claude web `conversations.json` (a **tree**). Path 3: the agent browses itself (Playwright MCP / CUA / Computer Use) — live actions, not history import.

**Why it exists.** Official whole-account export exists; live one-thread handoff is often unofficial and brittle. Beginners mash “I was on ChatGPT” with “let the agent open Chrome.”

**Mechanism.** Official ChatGPT export: Settings → Data Controls → Export Data → ZIP. `conversations.json` is `parent`/`child`, not API-linear `messages`. [Ch 30](./ch-30-chatgpt-handoff.md) is the tree → brief pipeline.

**Boundaries.** WebMCP exposes tools *from the page*. CUA *drives* the page. Export *copies history*. Three jobs.

**Failure modes.** Stuffing the tree into Cursor as L3. Treating an unofficial in-page scrape as a stable API.

## Comparison

| Path | Capture | What the agent gets |
| --- | --- | --- |
| Page context | extension reads DOM / selection / URL | markdown or accessibility tree of *this* page |
| Session handoff | ChatGPT/Claude web `conversations.json` | linearized messages + attachments + canvas |
| Agent browses itself | Playwright MCP / CUA / Computer Use | not history import — live actions |

**Read (读):**
- [docs — OpenAI export / data controls (in-product; tree ZIP, not linear messages).](https://help.openai.com/en/articles/7260999-how-do-i-export-my-chatgpt-history-and-data)
- [docs — OpenAI CUA: path 3, live computer use, not import.](https://openai.com/index/computer-using-agent/)
- [repo — microsoft/playwright-mcp: path 3 with element refs.](https://github.com/microsoft/playwright-mcp)
- [docs — Chrome WebMCP: page-exposed tools, not history export.](https://developer.chrome.com/docs/ai/webmcp/secure-tools)
- [docs — Anthropic Computer Use: another path-3 driver.](https://www.anthropic.com/news/3-5-models-and-computer-use)
- [docs — Claude.ai data export (current help): the Claude-side tree export analogue.](https://support.anthropic.com/en/articles/9451098-how-to-export-your-claude-data)

[Part VI](./README.md)
