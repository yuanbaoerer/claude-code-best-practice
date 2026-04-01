# 子代理最佳实践

![最后更新](https://img.shields.io/badge/最后更新-Mar%2028%2C%202026%206%3A00%20PM%20PKT-white?style=flat&labelColor=555)<br>
[![已实现](https://img.shields.io/badge/已实现-2ea44f?style=flat)](../implementation/claude-subagents-implementation.md)

Claude Code 子代理 — frontmatter 字段和官方内置代理类型。

<table width="100%">
<tr>
<td><a href="../">← 返回 Claude Code 最佳实践</a></td>
<td align="right"><img src="../!/claude-jumping.svg" alt="Claude" width="60" /></td>
</tr>
</table>

---

## Frontmatter 字段 (16)

| 字段 | 类型 | 必填 | 描述 |
|-------|------|----------|-------------|
| `name` | string | Yes | 使用小写字母和连字符的唯一标识符 |
| `description` | string | Yes | 何时调用。使用 `"PROACTIVELY"` 让 Claude 自动调用 |
| `tools` | string/list | No | 工具逗号分隔白名单（如 `Read, Write, Edit, Bash`）。省略时继承所有工具。支持 `Agent(agent_type)` 语法限制可生成的子代理；旧的 `Task(agent_type)` 别名仍有效 |
| `disallowedTools` | string/list | No | 要拒绝的工具，从继承或指定列表中移除 |
| `model` | string | No | 模型别名：`haiku`、`sonnet`、`opus` 或 `inherit`（默认：`inherit`） |
| `permissionMode` | string | No | 权限模式：`default`、`acceptEdits`、`dontAsk`、`bypassPermissions` 或 `plan` |
| `maxTurns` | integer | No | 子代理停止前的最大代理轮次数 |
| `skills` | list | No | 启动时预加载到代理上下文的技能名称（注入完整内容，不仅使其可用） |
| `mcpServers` | list | No | 此子代理的 MCP 服务器 — 服务器名称字符串或内联 `{name: config}` 对象 |
| `hooks` | object | No | 此子代理范围的生命周期钩子。支持所有钩子事件；`PreToolUse`、`PostToolUse` 和 `Stop` 最常见 |
| `memory` | string | No | 持久记忆范围：`user`、`project` 或 `local` |
| `background` | boolean | No | 设为 `true` 总是作为后台任务运行（默认：`false`） |
| `effort` | string | No | 此子代理激活时覆盖努力级别：`low`、`medium`、`high`、`max`。默认：从会话继承 |
| `isolation` | string | No | 设为 `"worktree"` 在临时 git worktree 中运行（无更改时自动清理） |
| `initialPrompt` | string | No | 当此代理作为主会话代理运行时（通过 `--agent` 或 `agent` 设置）自动提交为第一个用户轮次。命令和技能被处理。 prepend 到任何用户提供的提示词 |
| `color` | string | No | CLI 输出颜色用于视觉区分（如 `green`、`magenta`）。有效但未出现在官方 frontmatter 表 — 仅在交互式快速入门中文档 |

---

## ![官方](../!/tags/official.svg) **(6)**

| # | 代理 | 模型 | 工具 | 描述 |
|---|-------|-------|-------|-------------|
| 1 | `general-purpose` | inherit | All | 复杂多步任务 — 研究、代码搜索和自主工作的默认代理类型 |
| 2 | `Explore` | haiku | Read-only (no Write, Edit) | 快速代码库搜索和探索 — 优化用于查找文件、搜索代码和回答代码库问题 |
| 3 | `Plan` | inherit | Read-only (no Write, Edit) | 计划模式下的预规划研究 — 在编写代码前探索代码库并设计实现方案 |
| 4 | `Bash` | inherit | Bash | 在独立上下文中运行终端命令 |
| 5 | `statusline-setup` | sonnet | Read, Edit | 配置用户的 Claude Code 状态栏设置 |
| 6 | `claude-code-guide` | haiku | Glob, Grep, Read, WebFetch, WebSearch | 回答关于 Claude Code 功能、Agent SDK 和 Claude API 的问题 |

---

## 来源

- [Create custom subagents — Claude Code Docs](https://code.claude.com/docs/en/sub-agents)
- [CLI reference — Claude Code Docs](https://code.claude.com/docs/en/cli-reference)
- [Claude Code CHANGELOG](https://github.com/anthropics/claude-code/blob/main/CHANGELOG.md)