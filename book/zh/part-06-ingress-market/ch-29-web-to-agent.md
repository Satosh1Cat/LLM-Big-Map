# 第 29 章 · Web-to-Agent

把已经在浏览器里发生过的工作变成 agent 输入。三条**物理**路径——不要揉。

## 概念

**定义。** 路径 1：页面上下文——扩展读取*这一页*的 DOM / 选区 / URL。路径 2：session handoff——ChatGPT/Claude 网页的 `conversations.json`（一棵**树**）。路径 3：agent 自己逛（Playwright MCP / CUA / Computer Use）——现场动作，不是导入历史。

**为什么存在。** 官方整账户导出存在；活的一条线程交接常常非正式、易碎。初学者会把「我刚在 ChatGPT 上」和「让 agent 打开 Chrome」揉在一起。

**机制。** 官方 ChatGPT 导出：Settings → Data Controls → Export Data → ZIP。`conversations.json` 是 `parent`/`child`，不是 API 线性 `messages`。[第 30 章](./ch-30-chatgpt-handoff.md) 是树 → brief 管线。

**边界。** WebMCP 从*页面*暴露工具。CUA *驾驶*页面。导出*拷贝历史*。三件事。

**失败模式。** 把树当 L3 塞进 Cursor。把非正式的页内抓取当成稳定 API。

## 对比

| 路径 | 捕获 | Agent 得到什么 |
| --- | --- | --- |
| 页面上下文 | 扩展读 DOM / 选区 / URL | *这一页*的 markdown 或无障碍树 |
| Session handoff | ChatGPT/Claude 网页 `conversations.json` | 线性化消息 + 附件 + canvas |
| Agent 自己逛 | Playwright MCP / CUA / Computer Use | 不是导入历史——现场动作 |

**Read (读):**
- [docs — OpenAI 导出 / 数据控制（产品内；树状 ZIP，不是线性 messages）。](https://help.openai.com/en/articles/7260999-how-do-i-export-my-chatgpt-history-and-data)
- [docs — OpenAI CUA：路径 3，现场 computer use，不是导入。](https://openai.com/index/computer-using-agent/)
- [repo — microsoft/playwright-mcp：路径 3，元素引用。](https://github.com/microsoft/playwright-mcp)
- [docs — Chrome WebMCP：页面暴露的工具，不是历史导出。](https://developer.chrome.com/docs/ai/webmcp/secure-tools)
- [docs — Anthropic Computer Use：另一条路径 3 驱动器。](https://www.anthropic.com/news/3-5-models-and-computer-use)
- [docs — Claude.ai 数据导出（当前帮助）：Claude 侧的树状导出对应物。](https://support.anthropic.com/en/articles/9451098-how-to-export-your-claude-data)

[卷六](./README.md)
