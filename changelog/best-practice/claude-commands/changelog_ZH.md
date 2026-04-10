# 命令报告 — 变更历史

## 状态说明

| 状态 | 含义 |
|--------|---------|
| ✅ `COMPLETE (原因)` | 已采取行动并成功解决 |
| ❌ `INVALID (原因)` | 发现不正确、不适用或为有意的 |
| ✋ `ON HOLD (原因)` | 行动已推迟 — 等待外部依赖或用户决定 |

---

## [2026-03-13 04:23 PM PKT] Claude Code v2.1.74

| # | 优先级 | 类型 | 操作 | 状态 |
|---|----------|------|--------|--------|
| 1 | HIGH | 新字段 | 在 frontmatter 表中添加 `name` — 技能的显示名称 | ❌ INVALID（skill-only 字段，不适用于命令 frontmatter） |
| 2 | HIGH | 新字段 | 在 frontmatter 表中添加 `disable-model-invocation` — 防止自动加载 | ❌ INVALID（skill-only 字段，不适用于命令 frontmatter） |
| 3 | HIGH | 新字段 | 在 frontmatter 表中添加 `user-invocable` — 从 `/` 菜单隐藏 | ❌ INVALID（skill-only 字段，不适用于命令 frontmatter） |
| 4 | HIGH | 新字段 | 在 frontmatter 表中添加 `context` — fork 在子代理上下文中运行 | ❌ INVALID（skill-only 字段，不适用于命令 frontmatter） |
| 5 | HIGH | 新字段 | 在 frontmatter 表中添加 `agent` — context: fork 的子代理类型 | ❌ INVALID（skill-only 字段，不适用于命令 frontmatter） |
| 6 | HIGH | 新字段 | 在 frontmatter 表中添加 `hooks` — 限于技能的生命周期钩子 | ❌ INVALID（skill-only 字段，不适用于命令 frontmatter） |
| 7 | HIGH | 新命令 | 添加 `/btw <问题>` — 提出一个快速附带问题，不添加到对话中 | ✅ COMPLETE（添加为 Session 标签下的 #53） |
| 8 | HIGH | 新命令 | 添加 `/hooks` — 管理工具事件的钩子配置 | ✅ COMPLETE（添加为 Extensions 标签下的 #30） |
| 9 | HIGH | 新命令 | 添加 `/insights` — 生成会话分析报告 | ✅ COMPLETE（添加为 Context 标签下的 #17） |
| 10 | HIGH | 新命令 | 添加 `/plugin` — 管理 Claude Code 插件 | ✅ COMPLETE（添加为 Extensions 标签下的 #33） |
| 11 | HIGH | 新命令 | 添加 `/skills` — 列出可用技能 | ✅ COMPLETE（添加为 Extensions 标签下的 #35） |
| 12 | HIGH | 新命令 | 添加 `/upgrade` — 打开升级页面以切换计划层级 | ✅ COMPLETE（添加为 Auth 标签下的 #3） |
| 13 | HIGH | 已移除命令 | 移除 `/output-style` — 在 v2.1.73 中已弃用，请使用 `/config` | ✅ COMPLETE（已从 Config 标签中移除） |
| 14 | HIGH | 已移除命令 | 移除 `/bug` 行 — 现在作为 `/feedback` 的别名列出 | ✅ COMPLETE（已移除行，向 /feedback 描述添加了"别名: /bug"） |
| 15 | HIGH | 已修改描述 | 更新 `/passes` — 从审查通过改为推荐分享 | ✅ COMPLETE（已更新描述，保留在 Model 标签中） |
| 16 | HIGH | 已修改描述 | 更新 `/review` — 已弃用，由 `code-review` 市场插件替换 | ✅ COMPLETE（已更新 Project 标签中的描述） |
| 17 | MED | 已修改描述 | 更新 `/stickers` — 从 UI 贴纸包改为订购实体贴纸 | ✅ COMPLETE（已更新 Config 标签中的描述） |

---

## [2026-03-15 12:50 PM PKT] Claude Code v2.1.76

| # | 优先级 | 类型 | 操作 | 状态 |
|---|----------|------|--------|--------|
| 1 | HIGH | 新命令 | 向 Config 标签添加 `/color [颜色\|default]` — 设置当前会话的提示栏颜色 | ✅ COMPLETE（添加为 Config 标签下的 #4） |
| 2 | HIGH | 新命令 | 向 Model 标签添加 `/effort [low\|medium\|high\|max\|auto]` — 设置模型努力级别 | ✅ COMPLETE（添加为 Model 标签下的 #38） |
| 3 | MED | 已修改描述 | 更新 `/status` — 现在是"打开设置界面（状态标签）"而非"显示简洁的会话状态摘要" | ✅ COMPLETE（已在 Context 标签下的 #20 更新描述） |
| 4 | MED | 已修改描述 | 更新 `/desktop` — 现在是"在 Claude Code 桌面应用中继续当前会话。仅限 macOS 和 Windows。" | ✅ COMPLETE（已在 Remote 标签下的 #49 更新描述） |
| 5 | LOW | 已修改参数 | 更新 `/init` — 官方文档删除了 `[提示]` 参数提示 | ✅ COMPLETE（已在 Project 标签下的 #45 移除 [提示] 提示） |

---

## [2026-03-17 12:45 PM PKT] Claude Code v2.1.77

| # | 优先级 | 类型 | 操作 | 状态 |
|---|----------|------|--------|--------|
| 1 | HIGH | 新别名 | 向 `/fork` 条目添加`别名: /branch`（v2.1.77 将 fork 重命名为 branch） | ✅ COMPLETE（已向 Session 标签下的 /fork 添加"别名: /branch"，为 #59） |
| 2 | HIGH | 新别名 | 向 8 个命令添加别名: `/clear`（+/reset, /new）、`/config`（+/settings）、`/desktop`（+/app）、`/exit`（+/quit）、`/rewind`（+/checkpoint）、`/resume`（+/continue）、`/remote-control`（+/rc）、`/mobile`（+/ios, /android） | ✅ COMPLETE（已向所有 8 个命令描述添加别名标记） |
| 3 | MED | 已修改描述 | 更新 `/diff` — "打开交互式差异查看器，显示未提交的更改和每轮差异" | ✅ COMPLETE（已在 Project 标签下的 #44 更新描述） |
| 4 | MED | 已修改描述 | 更新 `/memory` — "编辑 CLAUDE.md 内存文件，启用或禁用自动记忆，以及查看自动记忆条目" | ✅ COMPLETE（已在 Memory 标签下的 #37 更新描述） |
| 5 | MED | 已修改描述 | 更新 `/copy` — "将上次助手响应复制到剪贴板。显示代码块的交互式选择器" | ✅ COMPLETE（已在 Export 标签下的 #27 更新描述） |
| 6 | MED | 已修改描述 | 更新 `/mobile` — "显示二维码以下载 Claude 移动应用" | ✅ COMPLETE（已在 Remote 标签下的 #52 更新描述和别名） |
| 7 | MED | 已修改描述 | 更新 `/remote-control` — "使此会话可从 claude.ai 进行远程控制" | ✅ COMPLETE（已在 Remote 标签下的 #53 更新描述和别名） |
| 8 | LOW | Frontmatter 范围 | 6 个 skill-only 字段仍缺失于报告中（有意的作用域划分） | ❌ INVALID（skill-only 字段 — 与 v2.1.74 运行时相同的判定） |

---

## [2026-03-18 11:38 PM PKT] Claude Code v2.1.78

| # | 优先级 | 类型 | 操作 | 状态 |
|---|----------|------|--------|--------|
| 1 | HIGH | 新命令 | 向 Config 标签添加 `/voice` — 切换按键通话语音听写 | ✅ COMPLETE（添加为 Config 标签下的 #15） |
| 2 | HIGH | 反转别名 | 将 `/fork` → `/branch` 互换主次，`/fork` 作为别名 | ✅ COMPLETE（已在 Session 标签下的 #56 互换为 `/branch`，按字母顺序重新排序） |
| 3 | MED | 新别名 | 向 `/permissions` 添加 `/allowed-tools` 别名 | ✅ COMPLETE（已向 Config 标签下的 #7 添加别名） |
| 4 | MED | 新参数 | 向 `/copy` 添加 `[N]` 参数语法 | ✅ COMPLETE（已在 Export 标签下的 #28 更新为 `/copy [N]`） |
| 5 | LOW | Frontmatter 范围 | 6 个 skill-only 字段缺失于报告中（有意的作用域划分） | ❌ INVALID（skill-only 字段 — 与 v2.1.74、v2.1.77 和 v2.1.78 运行时相同的判定） |

---

## [2026-03-19 11:54 AM PKT] Claude Code v2.1.79

| # | 优先级 | 类型 | 操作 | 状态 |
|---|----------|------|--------|--------|
| 1 | LOW | Frontmatter 范围 | 6 个 skill-only 字段缺失于报告中（有意的作用域划分） | ❌ INVALID（skill-only 字段 — 与 v2.1.74、v2.1.77、v2.1.78 运行时相同的判定） |

---

## [2026-03-20 08:33 AM PKT] Claude Code v2.1.80

| # | 优先级 | 类型 | 操作 | 状态 |
|---|----------|------|--------|--------|
| 1 | MED | 新字段 | 向 frontmatter 表添加 `effort` — 调用命令时覆盖模型努力级别（v2.1.80） | ✅ COMPLETE（添加为第 5 个字段，然后在完整字段集添加后重新定位到第 8 个） |
| 2 | HIGH | 质量检查修正 | 添加 6 个缺失字段（`name`、`disable-model-invocation`、`user-invocable`、`context`、`agent`、`hooks`）— 官方文档说明命令支持"与技能相同的 frontmatter"；之前的 INVALID 判定（v2.1.74–v2.1.79）是错误的 | ✅ COMPLETE（已添加全部 6 个字段，计数从 5 更新到 11，字段顺序与官方文档一致） |
| 3 | HIGH | 跨报告修复 | 向技能报告（`claude-skills.md`）添加 `effort` — 该字段在那里也缺失了 | ✅ COMPLETE（作为第 8 个字段添加到技能报告中，计数从 10 更新到 11） |

---

## [2026-03-21 09:08 PM PKT] Claude Code v2.1.81

无优先操作项 — 报告已与官方文档完全同步（11 个 frontmatter 字段，63 个内置命令）。

---

## [2026-03-23 09:48 PM PKT] Claude Code v2.1.81

无优先操作项 — 报告已与官方文档完全同步（11 个 frontmatter 字段，63 个内置命令）。

---

## [2026-03-25 08:07 PM PKT] Claude Code v2.1.83

| # | 优先级 | 类型 | 操作 | 状态 |
|---|----------|------|--------|--------|
| 1 | HIGH | 新命令 | 向 Remote 标签添加 `/schedule [描述]` — 创建、更新、列出或运行云计划任务 | ✅ COMPLETE（添加为 Remote 标签下的 #56，计数从 63 更新到 64） |

---

## [2026-03-26 01:01 PM PKT] Claude Code v2.1.84

| # | 优先级 | 类型 | 操作 | 状态 |
|---|----------|------|--------|--------|
| 1 | HIGH | 新字段 | 向 frontmatter 表添加 `shell` — `!command` 块的 shell（`bash` 或 `powershell`） | ✅ COMPLETE（添加为 `hooks` 前的第 12 个字段，计数从 11 更新到 12） |
| 2 | LOW | 已修改参数 | 向 `/fast` 命令添加 `[on\|off]` 参数提示 | ✅ COMPLETE（已在 Model 标签下的 #40 更新 `/fast` 为 `/fast [on\|off]`） |

---

## [2026-03-27 06:25 PM PKT] Claude Code v2.1.85

| # | 优先级 | 类型 | 操作 | 状态 |
|---|----------|------|--------|--------|
| 1 | HIGH | 新字段 | 向 frontmatter 表添加 `paths` — 限制技能激活的 glob 模式 | ✅ COMPLETE（添加为 `user-invocable` 后的第 6 个字段，计数从 12 更新到 13） |

---

## [2026-03-28 06:05 PM PKT] Claude Code v2.1.86

| # | 优先级 | 类型 | 操作 | 状态 |
|---|----------|------|--------|--------|
| 1 | MED | 已修改参数 | 更新 `/add-dir` — 按官方文档添加 `<路径>` 必需参数提示 | ✅ COMPLETE（已在 Project 标签下的 #44 更新） |
| 2 | MED | 已修改参数 | 更新 `/branch` — 按官方文档添加 `[名称]` 可选参数提示 | ✅ COMPLETE（已在 Session 标签下的 #57 更新） |
| 3 | MED | 已修改参数 | 更新 `/model` — 按官方文档添加 `[模型]` 可选参数提示 | ✅ COMPLETE（已在 Model 标签下的 #41 更新） |
| 4 | MED | 已修改参数 | 更新 `/plan` — 按官方文档添加 `[描述]` 可选参数提示 | ✅ COMPLETE（已在 Model 标签下的 #43 更新） |
| 5 | MED | 已修改参数 | 更新 `/pr-comments` — 按官方文档添加 `[PR]` 可选参数提示 | ✅ COMPLETE（已在 Project 标签下的 #47 更新） |
| 6 | MED | 已修改参数 | 更新 `/passes` — 移除 `[数字]` 参数提示（官方文档中不存在） | ✅ COMPLETE（已在 Model 标签下的 #42 更新） |
| 7 | MED | 已修改参数 | 更新 `/rename` — 按官方文档将参数从 `<名称>`（必需）改为 `[名称]`（可选） | ✅ COMPLETE（已在 Session 标签下的 #62 更新） |
| 8 | LOW | 已修改参数 | 更新 `/compact` — 按官方文档将参数标签从 `[提示]` 改为 `[指令]` | ✅ COMPLETE（已在 Session 标签下的 #60 更新） |
| 9 | LOW | 已修改参数 | 更新 `/feedback` — 按官方文档将参数标签从 `[描述]` 改为 `[报告]` | ✅ COMPLETE（已在 Debug 标签下的 #24 更新） |

---

## [2026-03-31 06:55 PM PKT] Claude Code v2.1.88

| # | 优先级 | 类型 | 操作 | 状态 |
|---|----------|------|--------|--------|
| 1 | MED | 描述同步 | 将全部 43 个命令描述与官方文档同步 — 行为说明（`/vim` 切换、`/sandbox` 切换、`/hooks` 查看）、扩展详情（`/effort` 持久化、`/copy` SSH 写入、`/model` 努力箭头）以及 Auth、Config、Context、Debug、Export、Extensions、Model、Project、Remote 和 Session 标签的措辞对齐 | ✅ COMPLETE（全部 64 个描述现在与 code.claude.com/docs/en/commands 上的官方文档一致） |

---

## [2026-04-01 12:26 PM PKT] Claude Code v2.1.89

| # | 优先级 | 类型 | 操作 | 状态 |
|---|----------|------|--------|--------|
| 1 | LOW | 已修改描述 | 更新 `/init` — 官方文档现在使用 `CLAUDE_CODE_NEW_INIT=1` 而非 `=true` | ✅ COMPLETE（已将环境变量值从 `=true` 更新为 `=1` 以匹配官方文档） |

---

## [2026-04-02 09:14 PM PKT] Claude Code v2.1.90

| # | 优先级 | 类型 | 操作 | 状态 |
|---|----------|------|--------|--------|
| 1 | MED | 已修改描述 | 更新 `/permissions` — 官方文档扩展了交互式对话框描述，包含作用域规则、目录管理和自动模式拒绝审查 | ✅ COMPLETE（已更新描述以匹配官方文档） |
| 2 | MED | 新别名 | 按官方文档向 `/tasks` 命令添加 `/bashes` 别名 | ✅ COMPLETE（已向 Debug 标签下的 #27 /tasks 添加"别名: /bashes"） |

---

## [2026-04-03 08:34 PM PKT] Claude Code v2.1.91

| # | 优先级 | 类型 | 操作 | 状态 |
|---|----------|------|--------|--------|
| 1 | HIGH | 新命令 | 向 Config 标签添加 `/powerup` — 通过带有动画演示的快速交互式课程发现 Claude Code 功能 | ✅ COMPLETE（添加为 Debug 标签下的 #26 — 在 v2.1.92 运行时解决） |

---

## [2026-04-04 10:40 PM PKT] Claude Code v2.1.92

| # | 优先级 | 类型 | 操作 | 状态 |
|---|----------|------|--------|--------|
| 1 | HIGH | 新命令 | 向 Debug 标签添加 `/powerup` — 通过带有动画演示的快速交互式课程发现 Claude Code 功能 | ✅ COMPLETE（添加为 Debug 标签下的 #26，延续自 v2.1.91） |
| 2 | HIGH | 新命令 | 向 Auth 标签添加 `/setup-bedrock` — 通过交互式向导配置 Amazon Bedrock 认证、区域和模型固定 | ✅ COMPLETE（添加为 Auth 标签下的 #3） |
| 3 | HIGH | 新命令 | 向 Model 标签添加 `/ultraplan <提示>` — 在 ultraplan 会话中起草计划，在浏览器中审查，然后远程执行或发回 | ✅ COMPLETE（添加为 Model 标签下的 #45） |
| 4 | HIGH | 已移除命令 | 从 Config 标签移除 `/vim` — 在 v2.1.92 中移除（max-version: 2.1.91），请使用 `/config` 编辑器模式代替 | ✅ COMPLETE（已从 Config 标签中移除） |
| 5 | HIGH | 已移除命令 | 从 Project 标签移除 `/pr-comments [PR]` — 在 v2.1.91 中移除（max-version: 2.1.90），请直接询问 Claude | ✅ COMPLETE（已从 Project 标签中移除） |
| 6 | MED | 已修改描述 | 更新 `/release-notes` — 现在是"在交互式版本选择器中查看变更日志。选择特定版本以查看其发布说明，或选择显示所有版本。" | ✅ COMPLETE（已在 Debug 标签下的 #27 更新描述） |

---

## [2026-04-08 09:35 PM PKT] Claude Code v2.1.96

无优先操作项 — 报告已与官方文档完全同步（13 个 frontmatter 字段，65 个内置命令）。

---

## [2026-04-09 11:31 PM PKT] Claude Code v2.1.97

| # | 优先级 | 类型 | 操作 | 状态 |
|---|----------|------|--------|--------|
| 1 | HIGH | 新命令 | 向 Remote 标签添加 `/autofix-pr [提示]` — 生成一个监视当前分支 PR 的网络会话，当 CI 失败或审阅者留下评论时推送修复 | ✅ COMPLETE（添加为 Remote 标签下的 #51，计数从 65 更新到 68） |
| 2 | HIGH | 新命令 | 向 Remote 标签添加 `/teleport` — 将网络会话中的 Claude Code 拉入此终端。别名: `/tp` | ✅ COMPLETE（添加为 Remote 标签下的 #59） |
| 3 | HIGH | 新命令 | 向 Remote 标签添加 `/web-setup` — 使用本地 `gh` CLI 凭证将 GitHub 账户连接到网络上的 Claude Code | ✅ COMPLETE（添加为 Remote 标签下的 #60） |
| 4 | MED | 已修改描述 | 更新 `/add-dir` — 官方文档现在包含关于从添加的目录中发现 `.claude/` 配置的警告 | ✅ COMPLETE（已在 Project 标签下的 #46 更新描述） |
