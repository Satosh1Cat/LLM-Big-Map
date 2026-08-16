# Ch 28 · Browser general agent

See the page, act on it. Context problem: screenshot tokens are expensive → prefer **external page state** + SELECTIVE_RETRIEVE, not dumping the DOM into the window.

## Concept

**Definition.** A browser / computer-use agent observes a GUI (pixels, accessibility tree, DOM, CDP) and acts (click, type, `exec_js`). Treat it as a high-risk tool. Permission layer: separate scopes for navigation domain, download, login jar. Page content is **untrusted** (indirect injection).

**Why it exists.** Many tasks are not in an API. CUA / Computer Use / Playwright MCP are three observation-action pairs, not one product.

**Mechanism.** Pixels + click coords (OpenAI CUA, Claude Computer Use, OSWorld). Accessibility / DOM snapshot + element refs (Playwright MCP default). CDP / `exec_js` for script-driven work. Hybrid: plugin API else pixels. CUA launch: WebVoyager 87% · WebArena 58.1% · OSWorld 38.1% (human 72.4%).

**Boundaries.** “Let the agent browse” is not “import ChatGPT history” ([Ch 29](../part-06-ingress-market/ch-29-web-to-agent.md)). Screenshot tokens fight compacting and cache. WebMCP is a page-exposed tool draft, not Playwright.

**Failure modes.** Dumping the DOM every turn. Reusing a login cookie across agents. Trusting page text as instructions.

## Comparison

| Observe | Act | Examples |
| --- | --- | --- |
| Screenshot pixels | click coords / keys | OpenAI CUA / computer tool; Claude Computer Use; OSWorld |
| Accessibility / DOM snapshot | click by element ref | Playwright MCP default |
| CDP structured | script-driven | Playwright / Selenium; CUA sample `exec_js` |
| Hybrid | plugin API else pixels | some 2026 desktop-control descriptions |

**Read (读):**
- [docs — OpenAI Computer-Using Agent launch: pixel + click; the 87 / 58.1 / 38.1 snapshot.](https://openai.com/index/computer-using-agent/)
- [docs — OpenAI computer use API guide (current).](https://platform.openai.com/docs/guides/tools-computer-use)
- [repo — openai/openai-cua-sample-app: sample app including `exec_js`.](https://github.com/openai/openai-cua-sample-app)
- [docs — Anthropic Computer Use announcement.](https://www.anthropic.com/news/3-5-models-and-computer-use)
- [repo — microsoft/playwright-mcp: accessibility-tree tools, not screenshots by default.](https://github.com/microsoft/playwright-mcp)
- [docs — Chrome WebMCP secure tools.](https://developer.chrome.com/docs/ai/webmcp/secure-tools)
- [repo — webmachinelearning/webmcp: WebMCP draft.](https://github.com/webmachinelearning/webmcp)
- [paper — OSWorld.](https://arxiv.org/abs/2404.07972)
- [paper — WebArena.](https://arxiv.org/abs/2307.13854)
- [paper — WebVoyager.](https://arxiv.org/abs/2401.13919)

[Part V](./README.md)
