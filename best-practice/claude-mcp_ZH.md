# MCP 服务器最佳实践

![最后更新](https://img.shields.io/badge/最后更新-Mar%2002%2C%202026%2012%3A30%20PM%20PKT-white?style=flat&labelColor=555)<br>
[![已实现](https://img.shields.io/badge/已实现-2ea44f?style=flat)](../.mcp.json)

MCP（模型上下文协议）服务器通过连接外部工具、数据库和 API 扩展 Claude Code。本指南涵盖日常使用的推荐服务器和配置最佳实践。

<table width="100%">
<tr>
<td><a href="../">← 返回 Claude Code 最佳实践</a></td>
<td align="right"><img src="../!/claude-jumping.svg" alt="Claude" width="60" /></td>
</tr>
</table>

---

## 日常使用的 MCP 服务器

> *"装了 15 个 MCP 服务器以为越多越好。结果日常只用 4 个。"* — [r/mcp](https://reddit.com/r/mcp/comments/1mj0fxs/) (682 upvotes)

| MCP 服务器 | 功能 | 资源 |
|------------|-------------|-----------|
| [**Context7**](https://github.com/upstash/context7) | 将最新库文档拉取到上下文。防止过时训练数据导致的幻觉 API | [Reddit: "by far the best MCP for coding"](https://reddit.com/r/mcp/comments/1qarjqm/) · [npm](https://www.npmjs.com/package/@upstash/context7-mcp) |
| [**Playwright**](https://github.com/microsoft/playwright-mcp) | 浏览器自动化 — 自主实现、测试和验证 UI 功能。截图、导航、表单测试 | [Reddit: essential for frontend](https://reddit.com/r/mcp/comments/1m59pk0/) · [Docs](https://playwright.dev/) |
| [**Claude in Chrome**](https://github.com/nicobailon/claude-code-in-chrome-mcp) | 连接 Claude 到你的真实 Chrome 浏览器 — 检查控制台、网络、DOM。调试用户实际看到的内容 | [Reddit: "game changer" for debugging](https://reddit.com/r/mcp/comments/1qarjqm/5_mcps_that_have_genuinely_made_me_10x_faster/nza0i7t/) · [对比报告](../reports/claude-in-chrome-v-chrome-devtools-mcp.md) |
| [**DeepWiki**](https://github.com/devanshusemwal/deepwiki-mcp) | 为任何 GitHub repo 获取结构化 wiki 风格文档 — 架构、API 接口、关系 | [Reddit: "put it behind a gateway with Context7"](https://reddit.com/r/mcp/comments/1qarjqm/) |
| [**Excalidraw**](https://github.com/antonpk1/excalidraw-mcp-app) | 从提示词生成架构图、流程图和系统设计，以手绘 Excalidraw 风格 | [GitHub](https://github.com/antonpk1/excalidraw-mcp-app) |

研究 (Context7/DeepWiki) -> 调试 (Playwright/Chrome) -> 文档 (Excalidraw)

---

## 配置

MCP 服务器在项目根目录的 `.mcp.json`（项目范围）或 `~/.claude.json`（用户范围）中配置。

### 服务器类型

| 类型 | 传输 | 示例 |
|------|-----------|---------|
| **stdio** | 生成本地进程 | `npx`、`python`、binary |
| **http** | 连接远程 URL | HTTP/SSE endpoint |

### 示例 `.mcp.json`

```json
{
  "mcpServers": {
    "context7": {
      "command": "npx",
      "args": ["-y", "@upstash/context7-mcp"]
    },
    "playwright": {
      "command": "npx",
      "args": ["-y", "@playwright/mcp"]
    },
    "deepwiki": {
      "command": "npx",
      "args": ["-y", "deepwiki-mcp"]
    },
    "remote-api": {
      "type": "http",
      "url": "https://mcp.example.com/mcp"
    }
  }
}
```

使用环境变量展开处理密钥，而不是在 `.mcp.json` 中提交 API key：

```json
{
  "mcpServers": {
    "remote-api": {
      "type": "http",
      "url": "https://mcp.example.com/mcp?token=${MCP_API_TOKEN}"
    }
  }
}
```

### MCP 服务器的设置

`.claude/settings.json` 中的这些设置控制 MCP 服务器审批：

| 键 | 类型 | 描述 |
|-----|------|-------------|
| `enableAllProjectMcpServers` | boolean | 无需提示自动审批所有 `.mcp.json` 服务器 |
| `enabledMcpjsonServers` | array | 特定服务器名称白名单自动审批 |
| `disabledMcpjsonServers` | array | 特定服务器名称黑名单拒绝 |

### MCP 工具的权限规则

MCP 工具在权限规则中遵循 `mcp__<server>__<tool>` 命名约定：

```json
{
  "permissions": {
    "allow": [
      "mcp__*",
      "mcp__context7__*",
      "mcp__playwright__browser_snapshot"
    ],
    "deny": [
      "mcp__dangerous-server__*"
    ]
  }
}
```

---

## MCP 范围

MCP 服务器可在三个层级定义：

| 范围 | 位置 | 用途 |
|-------|----------|---------|
| **项目** | `.mcp.json`（repo 根目录） | 团队共享服务器，提交到 git |
| **用户** | `~/.claude.json`（`mcpServers` 键） | 跨所有项目的个人服务器 |
| **子代理** | 代理 frontmatter（`mcpServers` 字段） | 特定子代理范围的服务器 |

优先级：子代理 > 项目 > 用户

---

## 来源

- [MCP Servers — Claude Code Docs](https://code.claude.com/docs/en/mcp)
- [Model Context Protocol Specification](https://modelcontextprotocol.io/)
- [5 MCPs that have genuinely made me 10x faster — r/mcp](https://reddit.com/r/mcp/comments/1qarjqm/)
- [MCP Server Overload Discussion — r/mcp](https://reddit.com/r/mcp/comments/1mj0fxs/)