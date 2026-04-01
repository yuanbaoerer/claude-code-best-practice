# 技能最佳实践

![最后更新](https://img.shields.io/badge/最后更新-Mar%2028%2C%202026%205%3A59%20PM%20PKT-white?style=flat&labelColor=555)<br>
[![已实现](https://img.shields.io/badge/已实现-2ea44f?style=flat)](../implementation/claude-skills-implementation.md)

Claude Code 技能 — frontmatter 字段和官方捆绑技能。

<table width="100%">
<tr>
<td><a href="../">← 返回 Claude Code 最佳实践</a></td>
<td align="right"><img src="../!/claude-jumping.svg" alt="Claude" width="60" /></td>
</tr>
</table>

---

## Frontmatter 字段 (13)

| 字段 | 类型 | 必填 | 描述 |
|-------|------|----------|-------------|
| `name` | string | No | 显示名称和 `/slash-command` 标识符。省略时默认为目录名 |
| `description` | string | Recommended | 技能的功能。显示在自动补全中，用于 Claude 自动发现 |
| `argument-hint` | string | No | 自动补全时显示的提示（如 `[issue-number]`、`[filename]`） |
| `disable-model-invocation` | boolean | No | 设为 `true` 阻止 Claude 自动调用此技能 |
| `user-invocable` | boolean | No | 设为 `false` 从 `/` 菜单隐藏 — 技能变为后台知识，用于代理预加载 |
| `allowed-tools` | string | No | 此技能激活时无需权限提示即可使用的工具 |
| `model` | string | No | 此技能运行时使用的模型（如 `haiku`、`sonnet`、`opus`） |
| `effort` | string | No | 调用时覆盖模型努力级别（`low`、`medium`、`high`、`max`） |
| `context` | string | No | 设为 `fork` 在隔离的子代理上下文中运行技能 |
| `agent` | string | No | 当 `context: fork` 设置时的子代理类型（默认：`general-purpose`） |
| `hooks` | object | No | 此技能范围的生命周期钩子 |
| `paths` | string/list | No | 限制技能自动激活时机的 glob 模式。接受逗号分隔字符串或 YAML 列表 — Claude 仅在处理匹配文件时加载技能 |
| `shell` | string | No | `` !`command` `` 块的 shell — `bash`（默认）或 `powershell`。需要 `CLAUDE_CODE_USE_POWERSHELL_TOOL=1` |

---

## ![官方](../!/tags/official.svg) **(5)**

| # | 技能 | 描述 |
|---|-------|-------------|
| 1 | `simplify` | 审查更改代码的复用、质量和效率 — 重构消除重复 |
| 2 | `batch` | 批量跨多个文件运行命令 |
| 3 | `debug` | 调试失败命令或代码问题 |
| 4 | `loop` | 按循环间隔运行提示词或斜杠命令（最长 3 天） |
| 5 | `claude-api` | 用 Claude API 或 Anthropic SDK 构建应用 — 在 `anthropic` / `@anthropic-ai/sdk` 导入时触发 |

另见：[官方技能仓库](https://github.com/anthropics/skills/tree/main/skills) 获取社区维护的可安装技能。

---

## 来源

- [Claude Code Skills — Docs](https://code.claude.com/docs/en/skills)
- [Skills Discovery in Monorepos](../reports/claude-skills-for-larger-mono-repos.md)
- [Claude Code CHANGELOG](https://github.com/anthropics/claude-code/blob/main/CHANGELOG.md)