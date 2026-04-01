# CLI 启动参数最佳实践

![最后更新](https://img.shields.io/badge/最后更新-Mar%2002%2C%202026-white?style=flat&labelColor=555)

Claude Code 启动参数、顶级子命令和启动环境变量的参考指南，用于从终端启动 Claude Code。

<table width="100%">
<tr>
<td><a href="../">← 返回 Claude Code 最佳实践</a></td>
<td align="right"><img src="../!/claude-jumping.svg" alt="Claude" width="60" /></td>
</tr>
</table>

---

## 目录

1. [会话管理](#会话管理)
2. [模型与配置](#模型与配置)
3. [权限与安全](#权限与安全)
4. [输出与格式](#输出与格式)
5. [系统提示词](#系统提示词)
6. [代理与子代理](#代理与子代理)
7. [MCP 与插件](#mcp-与插件)
8. [目录与工作区](#目录与工作区)
9. [预算与限制](#预算与限制)
10. [集成](#集成)
11. [初始化与维护](#初始化与维护)
12. [调试与诊断](#调试与诊断)
13. [设置覆盖](#设置覆盖)
14. [版本与帮助](#版本与帮助)
15. [子命令](#子命令)
16. [环境变量](#环境变量)

---

## 会话管理

| 参数 | 短参数 | 描述 |
|------|-------|-------------|
| `--continue` | `-c` | 继续当前目录中最近的对话 |
| `--resume` | `-r` | 通过 ID 或名称恢复特定会话，或显示交互式选择器 |
| `--from-pr <NUMBER\|URL>` | | 恢复链接到特定 GitHub PR 的会话 |
| `--fork-session` | | 恢复时创建新的会话 ID（与 `--resume` 或 `--continue` 配合使用） |
| `--session-id <UUID>` | | 使用特定的会话 ID（必须是有效的 UUID） |
| `--no-session-persistence` | | 禁用会话持久化（仅打印模式） |
| `--remote` | | 在 claude.ai 上创建新的网页会话 |
| `--teleport` | | 在本地终端恢复网页会话 |

---

## 模型与配置

| 参数 | 短参数 | 描述 |
|------|-------|-------------|
| `--model <NAME>` | | 使用别名（`sonnet`、`opus`、`haiku`）或完整模型 ID 设置模型 |
| `--fallback-model <NAME>` | | 默认模型过载时自动回退的模型（仅打印模式） |
| `--betas <LIST>` | | API 请求中包含的 beta headers（仅限 API key 用户） |

---

## 权限与安全

| 参数 | 短参数 | 描述 |
|------|-------|-------------|
| `--dangerously-skip-permissions` | | 跳过所有权限提示。请极度谨慎使用 |
| `--allow-dangerously-skip-permissions` | | 启用权限绕过选项但不激活它 |
| `--permission-mode <MODE>` | | 以指定权限模式启动：`default`、`plan`、`acceptEdits`、`bypassPermissions` |
| `--allowedTools <TOOLS>` | | 无需提示即可执行的工具（权限规则语法） |
| `--disallowedTools <TOOLS>` | | 从模型上下文中完全移除的工具 |
| `--tools <TOOLS>` | | 限制 Claude 可使用的内置工具（使用 `""` 禁用所有） |
| `--permission-prompt-tool <TOOL>` | | 指定 MCP 工具在非交互模式下处理权限提示 |

---

## 输出与格式

| 参数 | 短参数 | 描述 |
|------|-------|-------------|
| `--print` | `-p` | 无交互模式打印响应（无头/SDK 模式） |
| `--output-format <FORMAT>` | | 输出格式：`text`、`json`、`stream-json` |
| `--input-format <FORMAT>` | | 输入格式：`text`、`stream-json` |
| `--json-schema <SCHEMA>` | | 获取匹配 schema 的验证 JSON（仅打印模式） |
| `--include-partial-messages` | | 包含部分流式事件（需要 `--print` 和 `--output-format=stream-json`） |
| `--verbose` | | 启用详细日志，显示完整的逐轮输出 |

---

## 系统提示词

| 参数 | 短参数 | 描述 |
|------|-------|-------------|
| `--system-prompt <TEXT>` | | 用自定义文本替换整个系统提示词 |
| `--system-prompt-file <PATH>` | | 从文件加载系统提示词，替换默认值（仅打印模式） |
| `--append-system-prompt <TEXT>` | | 将自定义文本追加到默认系统提示词 |
| `--append-system-prompt-file <PATH>` | | 将文件内容追加到默认提示词（仅打印模式） |

---

## 代理与子代理

| 参数 | 短参数 | 描述 |
|------|-------|-------------|
| `--agent <NAME>` | | 为当前会话指定代理 |
| `--agents <JSON>` | | 通过 JSON 动态定义自定义子代理 |
| `--teammate-mode <MODE>` | | 设置代理团队显示：`auto`、`in-process`、`tmux` |

---

## MCP 与插件

| 参数 | 短参数 | 描述 |
|------|-------|-------------|
| `--mcp-config <PATH\|JSON>` | | 从 JSON 文件或字符串加载 MCP 服务器 |
| `--strict-mcp-config` | | 仅使用 `--mcp-config` 中的 MCP 服务器，忽略所有其他 |
| `--plugin-dir <PATH>` | | 仅为此会话从目录加载插件（可重复使用） |

---

## 目录与工作区

| 参数 | 短参数 | 描述 |
|------|-------|-------------|
| `--add-dir <PATH>` | | 为 Claude 添加额外的可访问工作目录 |
| `--worktree` | `-w` | 在隔离的 git worktree 中启动 Claude（从 HEAD 分支） |

---

## 预算与限制

| 参数 | 短参数 | 描述 |
|------|-------|-------------|
| `--max-budget-usd <AMOUNT>` | | API 调用停止前的最大美元金额（仅打印模式） |
| `--max-turns <NUMBER>` | | 限制代理轮次数量（仅打印模式） |

---

## 集成

| 参数 | 短参数 | 描述 |
|------|-------|-------------|
| `--chrome` | | 启用 Chrome 浏览器集成用于网页自动化 |
| `--no-chrome` | | 为此会话禁用 Chrome 浏览器集成 |
| `--ide` | | 启动时自动连接到 IDE（如果有且仅有一个有效 IDE） |

---

## 初始化与维护

| 参数 | 短参数 | 描述 |
|------|-------|-------------|
| `--init` | | 运行初始化钩子并启动交互模式 |
| `--init-only` | | 运行初始化钩子并退出（无交互会话） |
| `--maintenance` | | 运行维护钩子并退出 |

---

## 调试与诊断

| 参数 | 短参数 | 描述 |
|------|-------|-------------|
| `--debug <CATEGORIES>` | | 启用调试模式，可选类别过滤（如 `"api,hooks"`） |

---

## 设置覆盖

| 参数 | 短参数 | 描述 |
|------|-------|-------------|
| `--settings <PATH\|JSON>` | | 要加载的设置 JSON 文件路径或 JSON 字符串 |
| `--setting-sources <LIST>` | | 要加载的来源逗号分隔列表：`user`、`project`、`local` |
| `--disable-slash-commands` | | 为此会话禁用所有技能和斜杠命令 |

---

## 版本与帮助

| 参数 | 短参数 | 描述 |
|------|-------|-------------|
| `--version` | `-v` | 输出版本号 |
| `--help` | `-h` | 显示帮助信息 |

---

## 子命令

这些是作为 `claude <subcommand>` 运行的顶级命令：

| 子命令 | 描述 |
|------------|-------------|
| `claude` | 启动交互式 REPL |
| `claude "query"` | 以初始提示词启动 REPL |
| `claude agents` | 列出已配置的代理 |
| `claude auth` | 管理 Claude Code 认证 |
| `claude doctor` | 从命令行运行诊断 |
| `claude install` | 安装或切换 Claude Code 原生构建 |
| `claude mcp` | 配置 MCP 服务器（`add`、`remove`、`list`、`get`、`enable`） |
| `claude plugin` | 管理 Claude Code 插件 |
| `claude remote-control` | 管理远程控制会话 |
| `claude setup-token` | 为订阅使用创建长期令牌 |
| `claude update` / `claude upgrade` | 更新到最新版本 |

---

## 环境变量

这些启动专用环境变量在启动 Claude Code 之前在 shell 中设置（无法通过 `settings.json` 配置）：

| 变量 | 描述 |
|----------|-------------|
| `CLAUDE_CODE_EXPERIMENTAL_AGENT_TEAMS=1` | 启用实验性代理团队 |
| `CLAUDE_CODE_TMPDIR` | 覆盖内部文件的临时目录 |
| `CLAUDE_CODE_ADDITIONAL_DIRECTORIES_CLAUDE_MD=1` | 启用额外目录 CLAUDE.md 加载 |
| `DISABLE_AUTOUPDATER=1` | 禁用自动更新 |
| `CLAUDE_CODE_EFFORT_LEVEL` | 控制思考深度 — 参见 [设置参考](./claude-settings_ZH.md#环境变量-via-env) |
| `USE_BUILTIN_RIPGREP=0` | 使用系统 ripgrep 替代内置（Alpine Linux） |
| `CLAUDE_CODE_SIMPLE` | 启用简单模式（仅 Bash + Edit 工具） |
| `CLAUDE_BASH_NO_LOGIN=1` | BashTool 跳过登录 shell |

对于可通过 `settings.json` 中 `"env"` 键配置的环境变量（包括 `MAX_THINKING_TOKENS`、`CLAUDE_CODE_SHELL`、`CLAUDE_CODE_ENABLE_TASKS`、`CLAUDE_CODE_DISABLE_BACKGROUND_TASKS`、`CLAUDE_CODE_DISABLE_EXPERIMENTAL_BETAS` 等），参见 [Claude 设置参考](./claude-settings_ZH.md#环境变量-via-env)。

---

## 来源

- [Claude Code CLI Reference](https://code.claude.com/docs/en/cli-reference)
- [Claude Code Headless Mode](https://code.claude.com/docs/en/headless)
- [Claude Code Setup](https://code.claude.com/docs/en/setup)
- [Claude Code CHANGELOG](https://github.com/anthropics/claude-code/blob/main/CHANGELOG.md)
- [Claude Code Common Workflows](https://code.claude.com/docs/en/common-workflows)