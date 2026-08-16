# 第 28 章 · 浏览器 agent

看见页面，对它动手。上下文问题：截图像素 token 很贵 → 宁可 **窗外页面状态** + SELECTIVE_RETRIEVE，不要把 DOM 倒进窗口。

## 概念

**定义。** 浏览器 / computer-use agent 观察 GUI（像素、无障碍树、DOM、CDP）并行动（点击、打字、`exec_js`）。当高风险工具。权限层：导航域名、下载、登录 cookie 罐分开授权。页面内容**不可信**（间接注入）。

**为什么存在。** 很多任务不在 API 里。CUA / Computer Use / Playwright MCP 是三对观察-动作，不是一个产品。

**机制。** 像素 + 点击坐标（OpenAI CUA、Claude Computer Use、OSWorld）。无障碍 / DOM 快照 + 元素引用（Playwright MCP 默认）。CDP / `exec_js` 做脚本驱动。混合：有插件 API 就走 API，否则像素。CUA 发布：WebVoyager 87% · WebArena 58.1% · OSWorld 38.1%（人类 72.4%）。

**边界。** 「让 agent 自己逛」不是「导入 ChatGPT 历史」（[第 29 章](../part-06-ingress-market/ch-29-web-to-agent.md)）。截图 token 跟 compacting 和 cache 作对。WebMCP 是页面暴露工具的草案，不是 Playwright。

**失败模式。** 每轮倒 DOM。跨 agent 复用登录 cookie。把页面文字当指令信。

## 对比

| 观察 | 行动 | 例子 |
| --- | --- | --- |
| 截图像素 | 点击坐标 / 按键 | OpenAI CUA / computer tool；Claude Computer Use；OSWorld |
| 无障碍 / DOM 快照 | 按元素引用点 | Playwright MCP 默认 |
| CDP 结构化 | 脚本驱动 | Playwright / Selenium；CUA 样例 `exec_js` |
| 混合 | 有插件 API 否则像素 | 一些 2026 桌面控制描述 |

**Read (读):**
- [docs — OpenAI Computer-Using Agent 发布：像素 + 点击；87 / 58.1 / 38.1 快照。](https://openai.com/index/computer-using-agent/)
- [docs — OpenAI computer use API 指南（当前）。](https://platform.openai.com/docs/guides/tools-computer-use)
- [repo — openai/openai-cua-sample-app：含 `exec_js` 的样例应用。](https://github.com/openai/openai-cua-sample-app)
- [docs — Anthropic Computer Use 发布。](https://www.anthropic.com/news/3-5-models-and-computer-use)
- [repo — microsoft/playwright-mcp：默认无障碍树工具，不是截图。](https://github.com/microsoft/playwright-mcp)
- [docs — Chrome WebMCP secure tools。](https://developer.chrome.com/docs/ai/webmcp/secure-tools)
- [repo — webmachinelearning/webmcp：WebMCP 草案。](https://github.com/webmachinelearning/webmcp)
- [paper — OSWorld。](https://arxiv.org/abs/2404.07972)
- [paper — WebArena。](https://arxiv.org/abs/2307.13854)
- [paper — WebVoyager。](https://arxiv.org/abs/2401.13919)

[卷五](./README.md)
