# 命令最佳实践

![最后更新](https://img.shields.io/badge/最后更新-Mar%2028%2C%202026%206%3A05%20PM%20PKT-white?style=flat&labelColor=555)<br>
[![已实现](https://img.shields.io/badge/已实现-2ea44f?style=flat)](../implementation/claude-commands-implementation.md)

Claude Code 命令 — frontmatter 字段和官方内置斜杠命令。

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
| `description` | string | Recommended | 命令的功能。显示在自动补全中，用于 Claude 自动发现 |
| `argument-hint` | string | No | 自动补全时显示的提示（如 `[issue-number]`、`[filename]`） |
| `disable-model-invocation` | boolean | No | 设为 `true` 阻止 Claude 自动调用此命令 |
| `user-invocable` | boolean | No | 设为 `false` 从 `/` 菜单隐藏 — 命令变为后台知识 |
| `paths` | string/list | No | 限制此技能激活时的 glob 模式。接受逗号分隔字符串或 YAML 列表。设置后，Claude 仅在处理匹配模式的文件时自动加载技能 |
| `allowed-tools` | string | No | 此命令激活时无需权限提示即可使用的工具 |
| `model` | string | No | 此命令运行时使用的模型（如 `haiku`、`sonnet`、`opus`） |
| `effort` | string | No | 调用时覆盖模型努力级别（`low`、`medium`、`high`、`max`） |
| `context` | string | No | 设为 `fork` 在隔离的子代理上下文中运行命令 |
| `agent` | string | No | 当 `context: fork` 设置时的子代理类型（默认：`general-purpose`） |
| `shell` | string | No | `` !`command` `` 块的 shell — 接受 `bash`（默认）或 `powershell`。需要 `CLAUDE_CODE_USE_POWERSHELL_TOOL=1` |
| `hooks` | object | No | 此命令的范围生命周期钩子 |

---

## ![官方](../!/tags/official.svg) **(64)**

| # | 命令 | 标签 | 描述 |
|---|---------|-----|-------------|
| 1 | `/login` | ![认证](https://img.shields.io/badge/认证-2980B9?style=flat) | 通过 OAuth 认证 Claude Code |
| 2 | `/logout` | ![认证](https://img.shields.io/badge/认证-2980B9?style=flat) | 从 Claude Code 登出 |
| 3 | `/upgrade` | ![认证](https://img.shields.io/badge/认证-2980B9?style=flat) | 打开升级页面切换到更高计划等级 |
| 4 | `/color [color\|default]` | ![配置](https://img.shields.io/badge/配置-F39C12?style=flat) | 设置当前会话的提示栏颜色 |
| 5 | `/config` | ![配置](https://img.shields.io/badge/配置-F39C12?style=flat) | 打开设置界面调整主题、模型、输出风格等偏好。别名：`/settings` |
| 6 | `/keybindings` | ![配置](https://img.shields.io/badge/配置-F39C12?style=flat) | 每个上下文自定义键盘快捷键并创建和弦序列 |
| 7 | `/permissions` | ![配置](https://img.shields.io/badge/配置-F39C12?style=flat) | 查看或更新工具权限。别名：`/allowed-tools` |
| 8 | `/privacy-settings` | ![配置](https://img.shields.io/badge/配置-F39C12?style=flat) | 管理隐私和遥测偏好 |
| 9 | `/sandbox` | ![配置](https://img.shields.io/badge/配置-F39C12?style=flat) | 配置沙箱及依赖状态 |
| 10 | `/statusline` | ![配置](https://img.shields.io/badge/配置-F39C12?style=flat) | 设置 Claude Code 状态栏 UI |
| 11 | `/stickers` | ![配置](https://img.shields.io/badge/配置-F39C12?style=flat) | 订购 Claude Code 贴纸 |
| 12 | `/terminal-setup` | ![配置](https://img.shields.io/badge/配置-F39C12?style=flat) | 在 IDE 终端启用 shift+enter 换行 |
| 13 | `/theme` | ![配置](https://img.shields.io/badge/配置-F39C12?style=flat) | 更改颜色主题 |
| 14 | `/vim` | ![配置](https://img.shields.io/badge/配置-F39C12?style=flat) | 启用 vim 风格编辑模式 |
| 15 | `/voice` | ![配置](https://img.shields.io/badge/配置-F39C12?style=flat) | 切换按讲语音听写。需要 Claude.ai 账户 |
| 16 | `/context` | ![上下文](https://img.shields.io/badge/上下文-8E44AD?style=flat) | 将当前上下文使用情况可视化为带 token 计数的彩色网格 |
| 17 | `/cost` | ![上下文](https://img.shields.io/badge/上下文-8E44AD?style=flat) | 显示当前会话的 token 使用统计 |
| 18 | `/extra-usage` | ![上下文](https://img.shields.io/badge/上下文-8E44AD?style=flat) | 为订阅计划配置按用量计费的溢出 billing |
| 19 | `/insights` | ![上下文](https://img.shields.io/badge/上下文-8E44AD?style=flat) | 生成分析 Claude Code 会话的报告，包括项目区域、交互模式和摩擦点 |
| 20 | `/stats` | ![上下文](https://img.shields.io/badge/上下文-8E44AD?style=flat) | 可视化每日使用、会话历史、连续天数和模型偏好 |
| 21 | `/status` | ![上下文](https://img.shields.io/badge/上下文-8E44AD?style=flat) | 打开设置界面（状态标签）显示版本、模型、账户和连接状态 |
| 22 | `/usage` | ![上下文](https://img.shields.io/badge/上下文-8E44AD?style=flat) | 显示计划使用限制和速率限制状态（仅订阅计划） |
| 23 | `/doctor` | ![调试](https://img.shields.io/badge/调试-E74C3C?style=flat) | 检查 Claude Code 安装的健康状态 |
| 24 | `/feedback [report]` | ![调试](https://img.shields.io/badge/调试-E74C3C?style=flat) | 提交关于 Claude Code 的反馈。别名：`/bug` |
| 25 | `/help` | ![调试](https://img.shields.io/badge/调试-E74C3C?style=flat) | 显示斜杠命令帮助 |
| 26 | `/release-notes` | ![调试](https://img.shields.io/badge/调试-E74C3C?style=flat) | 显示最近的 Claude Code 发行说明 |
| 27 | `/tasks` | ![调试](https://img.shields.io/badge/调试-E74C3C?style=flat) | 列出和管理后台任务 |
| 28 | `/copy [N]` | ![导出](https://img.shields.io/badge/导出-7F8C8D?style=flat) | 复制最近（或第 N 近）的助手响应到剪贴板。代码块显示交互式选择器 |
| 29 | `/export [filename]` | ![导出](https://img.shields.io/badge/导出-7F8C8D?style=flat) | 将当前对话导出到文件或剪贴板 |
| 30 | `/agents` | ![扩展](https://img.shields.io/badge/扩展-16A085?style=flat) | 管理自定义子代理 — 查看、创建、编辑、删除 |
| 31 | `/chrome` | ![扩展](https://img.shields.io/badge/扩展-16A085?style=flat) | 管理 Claude in Chrome 浏览器集成 |
| 32 | `/hooks` | ![扩展](https://img.shields.io/badge/扩展-16A085?style=flat) | 管理工具事件的钩子配置 |
| 33 | `/ide` | ![扩展](https://img.shields.io/badge/扩展-16A085?style=flat) | 连接到 IDE 集成 |
| 34 | `/mcp` | ![扩展](https://img.shields.io/badge/扩展-16A085?style=flat) | 管理 MCP 服务器连接 |
| 35 | `/plugin` | ![扩展](https://img.shields.io/badge/扩展-16A085?style=flat) | 管理 Claude Code 插件 |
| 36 | `/reload-plugins` | ![扩展](https://img.shields.io/badge/扩展-16A085?style=flat) | 不重启即可重新加载已安装的插件 |
| 37 | `/skills` | ![扩展](https://img.shields.io/badge/扩展-16A085?style=flat) | 列出可用技能 |
| 38 | `/memory` | ![记忆](https://img.shields.io/badge/记忆-3498DB?style=flat) | 编辑 CLAUDE.md 记忆文件、启用或禁用自动记忆、查看自动记忆条目 |
| 39 | `/effort [low\|medium\|high\|max\|auto]` | ![模型](https://img.shields.io/badge/模型-E67E22?style=flat) | 设置模型努力级别 |
| 40 | `/fast [on\|off]` | ![模型](https://img.shields.io/badge/模型-E67E22?style=flat) | 切换快速模式 — 同样使用 Opus 4.6 模型但输出更快 |
| 41 | `/model [model]` | ![模型](https://img.shields.io/badge/模型-E67E22?style=flat) | 选择或更改 AI 模型 |
| 42 | `/passes` | ![模型](https://img.shields.io/badge/模型-E67E22?style=flat) | 与朋友分享一周免费的 Claude Code。仅当账户符合条件时可见 |
| 43 | `/plan [description]` | ![模型](https://img.shields.io/badge/模型-E67E22?style=flat) | 直接从提示词进入计划模式。传递可选描述立即以该任务开始 |
| 44 | `/add-dir <path>` | ![项目](https://img.shields.io/badge/项目-27AE60?style=flat) | 向当前会话添加新的工作目录 |
| 45 | `/diff` | ![项目](https://img.shields.io/badge/项目-27AE60?style=flat) | 打开交互式 diff 查看器显示未提交更改和逐轮 diff |
| 46 | `/init` | ![项目](https://img.shields.io/badge/项目-27AE60?style=flat) | 用 CLAUDE.md 指南初始化新项目 |
| 47 | `/pr-comments [PR]` | ![项目](https://img.shields.io/badge/项目-27AE60?style=flat) | 获取并显示 GitHub pull request 的评论。自动检测当前分支的 PR，或传递 PR URL 或编号 |
| 48 | `/review` | ![项目](https://img.shields.io/badge/项目-27AE60?style=flat) | 已弃用 — 安装 `code-review` 插件替代 |
| 49 | `/security-review` | ![项目](https://img.shields.io/badge/项目-27AE60?style=flat) | 对当前更改运行针对性安全审查 |
| 50 | `/desktop` | ![远程](https://img.shields.io/badge/远程-5D6D7E?style=flat) | 在 Claude Code Desktop 应用中继续当前会话。仅 macOS 和 Windows。别名：`/app` |
| 51 | `/install-github-app` | ![远程](https://img.shields.io/badge/远程-5D6D7E?style=flat) | 安装 GitHub 应用用于 PR 链接工作流 |
| 52 | `/install-slack-app` | ![远程](https://img.shields.io/badge/远程-5D6D7E?style=flat) | 安装 Slack 应用用于通知和分享 |
| 53 | `/mobile` | ![远程](https://img.shields.io/badge/远程-5D6D7E?style=flat) | 显示二维码下载 Claude 移动应用。别名：`/ios`、`/android` |
| 54 | `/remote-control` | ![远程](https://img.shields.io/badge/远程-5D6D7E?style=flat) | 使此会话可从 claude.ai 远程控制。别名：`/rc` |
| 55 | `/remote-env` | ![远程](https://img.shields.io/badge/远程-5D6D7E?style=flat) | 检查或复制远程控制环境设置 |
| 56 | `/schedule [description]` | ![远程](https://img.shields.io/badge/远程-5D6D7E?style=flat) | 创建、更新、列出或运行 Cloud 计划任务。Claude 会对话式引导你完成设置 |
| 57 | `/branch [name]` | ![会话](https://img.shields.io/badge/会话-4A90D9?style=flat) | 在当前点创建对话分支。别名：`/fork` |
| 58 | `/btw <question>` | ![会话](https://img.shields.io/badge/会话-4A90D9?style=flat) | 提快速侧边问题而不添加到对话中 |
| 59 | `/clear` | ![会话](https://img.shields.io/badge/会话-4A90D9?style=flat) | 清除对话历史并释放上下文。别名：`/reset`、`/new` |
| 60 | `/compact [instructions]` | ![会话](https://img.shields.io/badge/会话-4A90D9?style=flat) | 带可选聚焦指令压缩对话 |
| 61 | `/exit` | ![会话](https://img.shields.io/badge/会话-4A90D9?style=flat) | 退出 CLI。别名：`/quit` |
| 62 | `/rename [name]` | ![会话](https://img.shields.io/badge/会话-4A90D9?style=flat) | 重命名当前会话。无名称时从对话历史自动生成 |
| 63 | `/resume [session]` | ![会话](https://img.shields.io/badge/会话-4A90D9?style=flat) | 通过 ID 或名称恢复之前的对话，或打开会话选择器。别名：`/continue` |
| 64 | `/rewind` | ![会话](https://img.shields.io/badge/会话-4A90D9?style=flat) | 回退对话和/或代码到之前的点，或从选定消息总结。别名：`/checkpoint` |

捆绑技能如 `/debug` 也会出现在斜杠命令菜单中，但它们不是内置命令。

---

## 来源

- [Claude Code Slash Commands](https://code.claude.com/docs/en/slash-commands)
- [Claude Code Interactive Mode](https://code.claude.com/docs/en/interactive-mode)
- [Claude Code CHANGELOG](https://github.com/anthropics/claude-code/blob/main/CHANGELOG.md)