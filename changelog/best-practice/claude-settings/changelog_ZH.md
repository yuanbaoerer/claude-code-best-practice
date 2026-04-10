# 设置报告 — 变更日志历史

## 状态图例

| 状态 | 含义 |
|--------|---------|
| ✅ `COMPLETE (原因)` | 操作已采取并成功解决 |
| ❌ `INVALID (原因)` | 发现不正确、不适用或为有意为之 |
| ✋ `ON HOLD (原因)` | 操作已推迟 — 等待外部依赖或用户决定 |

---

## [2026-03-05 06:18 AM PKT] Claude Code v2.1.69

| # | 优先级 | 类型 | 操作 | 状态 |
|---|----------|------|--------|--------|
| 1 | HIGH | 缺失设置项 | 添加13个非钩子缺失的设置项（`$schema`、`availableModels`、`fastModePerSessionOptIn`、`teammateMode`、`prefersReducedMotion`、`sandbox.filesystem.*`、`sandbox.network.allowManagedDomainsOnly`、`sandbox.enableWeakerNetworkIsolation`、`allowManagedMcpServersOnly`、`blockedMarketplaces`、`includeGitInstructions`、`pluginTrustMessage`、`fileSuggestion`） | ✅ COMPLETE（已添加到报告） |
| 2 | HIGH | 缺失环境变量 | 添加缺失的环境变量，包括 `CLAUDE_CODE_DISABLE_ADAPTIVE_THINKING`、`CLAUDE_CODE_DISABLE_1M_CONTEXT`、`CLAUDE_CODE_ACCOUNT_UUID`、`CLAUDE_CODE_DISABLE_GIT_INSTRUCTIONS`、`ENABLE_CLAUDEAI_MCP_SERVERS` 等 | ✅ COMPLETE（已添加13个缺失的环境变量到报告） |
| 3 | HIGH | 努力级别默认值 | 将努力级别默认值从"High"更新为"Medium"（Max/Team订阅者）；添加 Sonnet 4.6 支持（v2.1.68 变更） | ✅ COMPLETE（已更新默认值并添加 Sonnet 说明） |
| 4 | MED | 设置层级 | 添加通过 macOS plist/Windows Registry 的托管设置（v2.1.61/v2.1.69）；记录跨作用域的数组合并行为 | ✅ COMPLETE（已添加 plist/Registry 和合并说明） |
| 5 | MED | 沙盒文件系统 | 添加 `sandbox.filesystem.allowWrite`、`denyWrite`、`denyRead`，路径前缀语义（`//`、`~/`、`/`、`./`） | ✅ COMPLETE（已添加到沙盒表） |
| 6 | MED | 权限语法 | 添加 `Agent(name)` 权限模式；记录 `MCP(server:tool)` 语法形式 | ✅ COMPLETE（已添加到工具语法表） |
| 7 | MED | 插件缺口 | 添加 `blockedMarketplaces`、`pluginTrustMessage` | ✅ COMPLETE（已添加到插件表） |
| 8 | MED | 模型配置 | 添加 `availableModels` 设置项 | ✅ COMPLETE（已添加到常规设置表） |
| 9 | MED | 可疑键名 | 验证 `sandbox.network.deniedDomains`、`sandbox.ignoreViolations`、`pluginConfigs` — 报告中存在但官方文档中无 | ✋ ON HOLD（保留在报告中等待验证） |
| 10 | LOW | 标题计数 | 将标题从"38个设置项和84个环境变量"更新为实际数量（~55+设置项、~110+环境变量） | ✅ COMPLETE（已更新标题） |
| 11 | LOW | CLAUDE.md 同步 | 更新 CLAUDE.md 配置层级（添加 managed/CLI/user 级别） | ✋ ON HOLD（等待用户批准） |
| 12 | LOW | 示例更新 | 使用 `$schema`、沙盒文件系统、`Agent(*)` 更新快速参考示例，移除钩子示例 | ✅ COMPLETE（已更新示例） |
| 13 | MED | 钩子重定向 | 将钩子部分替换为指向 claude-code-hooks 仓库的重定向 | ✅ COMPLETE（钩子已外部化到专用仓库） |

---

## [2026-03-07 02:17 PM PKT] Claude Code v2.1.71

| # | 优先级 | 类型 | 操作 | 状态 |
|---|----------|------|--------|--------|
| 1 | HIGH | 行为变更 | 修复 `teammateMode`：类型 `boolean` → `string`，默认值 `false` → `"auto"`，描述 → "代理团队显示：auto、in-process、tmux" | ✅ COMPLETE（类型、默认值和描述已更新） |
| 2 | HIGH | 新设置项 | 添加 `allowManagedPermissionRulesOnly` 到权限表（boolean，仅托管） | ✅ COMPLETE（已添加到权限键表） |
| 3 | HIGH | 缺失环境变量 | 添加约31个缺失的环境变量，包括已确认的（`CLAUDE_CODE_MAX_OUTPUT_TOKENS`、`CLAUDE_CODE_DISABLE_FAST_MODE`、`CLAUDE_CODE_DISABLE_AUTO_MEMORY`、`CLAUDE_CODE_USER_EMAIL`、`CLAUDE_CODE_ORGANIZATION_UUID`、`CLAUDE_CONFIG_DIR`）和代理报告的（Foundry、Bedrock、mTLS、shell 前缀等） | ✅ COMPLETE（已添加31个环境变量到表） |
| 4 | MED | 默认值变更 | 修复 `plansDirectory` 默认值从 `.claude/plans/` 到 `~/.claude/plans` | ✅ COMPLETE（默认值已更新） |
| 5 | MED | 描述变更 | 修复 `sandbox.enableWeakerNetworkIsolation` 描述为"（仅 macOS）允许访问系统 TLS 信任；降低安全性" | ✅ COMPLETE（描述已更新） |
| 6 | MED | 作用域修复 | 修复 `extraKnownMarketplaces` 作用域从"Any"到"Project" | ✅ COMPLETE（作用域和描述已更新） |
| 7 | MED | 边界违规 | 替换 `claude-cli-startup-flags.md` 中的 `CLAUDE_CODE_EFFORT_LEVEL`，添加对设置报告的交叉引用 | ✅ COMPLETE（替换为链接） |
| 8 | MED | 版本徽章 | 将报告版本从 v2.1.69 更新到 v2.1.71 | ✅ COMPLETE（徽章和标题已更新） |
| 9 | LOW | 可疑键名 | 验证 `skipWebFetchPreflight`、`sandbox.ignoreViolations`、`sandbox.network.deniedDomains`、`skippedMarketplaces`、`skippedPlugins`、`pluginConfigs` | ✋ ON HOLD（保留在报告中等待验证 — 自 2026-03-05 起 recurring） |
| 10 | LOW | CLAUDE.md 同步 | 更新 CLAUDE.md 配置层级（3级 → 5+级） | ✅ COMPLETE（已更新为5级层级结构，含托管层） |

---

## [2026-03-12 12:23 PM PKT] Claude Code v2.1.74

| # | 优先级 | 类型 | 操作 | 状态 |
|---|----------|------|--------|--------|
| 1 | HIGH | 行为变更 | 修复 `dontAsk` 权限模式描述："自动接受所有工具" → "自动拒绝工具，除非通过 `/permissions` 或 `permissions.allow` 规则预先批准" | ✅ COMPLETE（描述已根据官方权限文档修正） |
| 2 | HIGH | 新设置项 | 添加 `modelOverrides` 到模型配置部分（对象，将 Anthropic 模型 ID 映射到提供商特定 ID 如 Bedrock ARN） | ✅ COMPLETE（已添加，含示例和描述） |
| 3 | HIGH | 新设置项 | 添加 `allow_remote_sessions` 到仅托管设置项列表（boolean，默认为 `true`，控制远程控制/网络会话访问） | ✅ COMPLETE（已添加到权限键表） |
| 4 | HIGH | 默认值变更 | 修复 `$schema` URL 从 `https://www.schemastore.org/...` 到 `https://json.schemastore.org/...`（根据官方文档） | ✅ COMPLETE（已在描述、示例和来源中更新） |
| 5 | MED | 描述变更 | 修复 `ANTHROPIC_CUSTOM_HEADERS` 格式描述从"JSON 字符串"到"Name: Value 格式，逗号分隔" | ✅ COMPLETE（描述已根据官方文档更新） |
| 6 | MED | 未验证模式 | `askEdits` 和 `viewOnly` 权限模式不在官方文档中 — 仅记录了5种模式（default、acceptEdits、plan、dontAsk、bypassPermissions） | ✅ COMPLETE（在表中标记为"官方文档中无 — 未验证"） |
| 7 | MED | 缺失环境变量 | 添加 `CLAUDE_CODE_SESSIONEND_HOOKS_TIMEOUT_MS`、`CLAUDE_CODE_DISABLE_FEEDBACK_SURVEY`、`CLAUDE_CODE_DISABLE_TERMINAL_TITLE`、`CLAUDE_CODE_IDE_SKIP_AUTO_INSTALL`、`CLAUDE_CODE_OTEL_HEADERS_HELPER_DEBOUNCE_MS` | ✅ COMPLETE（已添加5个环境变量） |
| 8 | MED | 新设置项 | 添加 `autoMemoryDirectory` 到核心配置（字符串，自定义自动记忆目录）— 版本不确定（代理间有分歧：v2.1.68 vs v2.1.74），不在设置页面上 | ✅ COMPLETE（已添加在 plansDirectory 附近 — 版本未解决） |
| 9 | LOW | 可疑键名 | 验证 `skipWebFetchPreflight`、`sandbox.ignoreViolations`、`sandbox.network.deniedDomains`、`skippedMarketplaces`、`skippedPlugins`、`pluginConfigs` — 仍不在官方文档中 | ✋ ON HOLD（保留在报告中等待验证 — 自 2026-03-05 起 recurring） |
| 10 | LOW | 缺失环境变量 | 添加 `CLAUDE_CODE_SUBAGENT_MODEL` 到环境变量表（已在模型环境示例块中但表中缺失） | ✅ COMPLETE（已添加到环境变量表） |
| 11 | LOW | 示例更新 | 更新快速参考示例以包含 `modelOverrides` 和修正的 `$schema` URL | ✅ COMPLETE（示例已更新） |

---

## [2026-03-14 01:35 AM PKT] Claude Code v2.1.75

| # | 优先级 | 类型 | 操作 | 状态 |
|---|----------|------|--------|--------|
| 1 | HIGH | 设置层级 | 重构以匹配官方5级层级：托管 (#1) > CLI 参数 > 本地 > 项目 > 用户。移除 `~/.claude/settings.local.json` 行。添加托管层级内部优先级（server-managed > MDM > 文件 > HKCU）。说明托管"无法被任何其他级别覆盖，包括 CLI 参数" | ✅ COMPLETE（已重构为5级，托管为 #1，添加了传递方法、内部优先级和文件路径） |
| 2 | HIGH | 行为变更 | 修复 `availableModels` 描述：从复杂对象数组（`title`/`modelId`/`effortOptions`）改为简单字符串数组 `["sonnet", "haiku"]`（根据官方文档） | ✅ COMPLETE（描述已更新以匹配官方文档格式） |
| 3 | HIGH | 行为变更 | 添加 `cleanupPeriodDays` `0` 值行为："设置为 `0` 在启动时删除所有现有 transcripts 并完全禁用会话持久化" | ✅ COMPLETE（已在描述中添加0值行为） |
| 4 | HIGH | 权限语法 | 在权限部分添加评估顺序说明："规则按顺序评估：先拒绝规则，再询问，再允许。第一个匹配的规则生效。" | ✅ COMPLETE（已在 Bash 通配符说明前添加评估顺序） |
| 5 | MED | 描述变更 | 添加 `autoMemoryDirectory` 作用域限制："不接受项目设置（`.claude/settings.json`）。接受来自策略、本地和用户设置" | ✅ COMPLETE（已在描述中添加作用域限制） |
| 6 | MED | 描述变更 | 添加 `permissions.defaultMode` 远程环境说明：仅 `acceptEdits` 和 `plan` 在远程环境中有效（v2.1.70） | ✅ COMPLETE（已在描述中添加远程限制） |
| 7 | MED | 模型配置 | 添加 Opus 4.6 1M 上下文默认值说明：自 v2.1.75 起，1M 上下文是 Max/Team/Enterprise 计划的默认值 | ✅ COMPLETE（已添加到努力级别说明） |
| 8 | MED | 设置层级 | 添加 Windows 托管路径说明：v2.1.75 移除了已弃用的 `C:\ProgramData\ClaudeCode\` 回退 — 使用 `C:\Program Files\ClaudeCode\managed-settings.json` | ✅ COMPLETE（已在层级部分添加弃用说明） |
| 9 | MED | 显示与体验 | 添加 `fileSuggestion` stdin JSON 格式（`{"query": "..."}`）和15路径输出限制说明 | ✅ COMPLETE（已在文件建议部分添加 stdin 格式和输出限制） |
| 10 | MED | 设置层级 | 将数组合并说明从"已合并"更新为"已连接和去重"（根据官方文档） | ✅ COMPLETE（已在层级重要部分更新措辞） |
| 11 | LOW | 可疑键名 | `sandbox.ignoreViolations`、`sandbox.network.deniedDomains` 仍不在官方文档或 JSON schema 顶层中 | ✋ ON HOLD（保留在报告中等待验证 — 自 2026-03-05 起 recurring） |
| 12 | LOW | 可疑键名 | `skipWebFetchPreflight`、`skippedMarketplaces`、`skippedPlugins`、`pluginConfigs` — 在 JSON schema 中确认但不在官方设置页面上 | ✋ ON HOLD（保留在报告中 — 根据 schema 有效，自 2026-03-05 起 recurring） |

---

## [2026-03-15 12:52 PM PKT] Claude Code v2.1.76

| # | 优先级 | 类型 | 操作 | 状态 |
|---|----------|------|--------|--------|
| 1 | HIGH | 新设置项 | 添加 `effortLevel` 到常规设置或模型配置 — 跨会话保持努力级别（`"low"`、`"medium"`、`"high"`）。在官方设置页面上确认 | ✋ ON HOLD（等待用户批准） |
| 2 | HIGH | 新设置项 | 添加工作树设置部分，含 `worktree.sparsePaths`（数组，稀疏检出锥模式）和 `worktree.symlinkDirectories`（数组，要去重的符号链接目录）。在官方设置页面上确认 | ✋ ON HOLD（等待用户批准） |
| 3 | HIGH | 新设置项 | 添加 `feedbackSurveyRate` 到常规设置 — 会话质量调查的概率（0-1）。在官方设置页面上确认 | ✋ ON HOLD（等待用户批准） |
| 4 | HIGH | 缺失环境变量 | 添加23个缺失的环境变量到表：`CLAUDE_CODE_AUTO_COMPACT_WINDOW`、`CLAUDE_CODE_ENABLE_PROMPT_SUGGESTION`、`CLAUDE_CODE_PLAN_MODE_REQUIRED`、`CLAUDE_CODE_TEAM_NAME`、`CLAUDE_CODE_TASK_LIST_ID`、`CLAUDE_ENV_FILE`、`FORCE_AUTOUPDATE_PLUGINS`、`HTTP_PROXY`、`HTTPS_PROXY`、`NO_PROXY`、`MCP_TOOL_TIMEOUT`、`MCP_CLIENT_SECRET`、`MCP_OAUTH_CALLBACK_PORT`、`IS_DEMO`、`SLASH_COMMAND_TOOL_CHAR_BUDGET`、`VERTEX_REGION_CLAUDE_3_5_HAIKU`、`VERTEX_REGION_CLAUDE_3_7_SONNET`、`VERTEX_REGION_CLAUDE_4_0_OPUS`、`VERTEX_REGION_CLAUDE_4_0_SONNET`、`VERTEX_REGION_CLAUDE_4_1_OPUS`。在官方 /en/env-vars 页面确认 | ✋ ON HOLD（等待用户批准） |
| 5 | HIGH | 缺失环境变量 | 将 `ANTHROPIC_DEFAULT_OPUS_MODEL`、`ANTHROPIC_DEFAULT_SONNET_MODEL`、`MAX_THINKING_TOKENS` 从代码块移到通用环境变量表 | ✋ ON HOLD（等待用户批准） |
| 6 | HIGH | 失效链接 | 修复 `https://claudelog.com/configuration/` — 返回 ECONNREFUSED。移除或替换为有效来源 | ✋ ON HOLD（等待用户批准） |
| 7 | MED | 描述变更 | 更新 `cleanupPeriodDays` 描述以添加：设置为0时"钩子收到空的 `transcript_path`"。根据官方文档 | ✋ ON HOLD（等待用户批准） |
| 8 | MED | 未验证环境变量 | 将报告中存在但不在官方文档中的7个环境变量标记为未验证：`CLAUDE_CODE_DISABLE_MCP`、`CLAUDE_CODE_DISABLE_TOOLS`、`CLAUDE_CODE_HIDE_ACCOUNT_INFO`、`CLAUDE_CODE_MAX_TURNS`、`CLAUDE_CODE_PROMPT_CACHING_ENABLED`、`CLAUDE_CODE_SKIP_SETTINGS_SETUP`、`DISABLE_NON_ESSENTIAL_MODEL_CALLS` | ✋ ON HOLD（等待用户批准） |
| 9 | MED | 新来源 | 添加 `https://code.claude.com/docs/en/env-vars` 到来源部分 — 官方环境变量参考页面 | ✋ ON HOLD（等待用户批准） |
| 10 | MED | 示例更新 | 更新快速参考示例以包含 `effortLevel` 和 `worktree` 设置项 | ✋ ON HOLD（等待用户批准） |
| 11 | LOW | 可疑键名 | `sandbox.ignoreViolations`、`sandbox.network.deniedDomains` 仍不在官方文档沙盒表中 | ✋ ON HOLD（保留在报告中等待验证 — 自 2026-03-05 起 recurring） |
| 12 | LOW | 可疑键名 | `skipWebFetchPreflight`、`skippedMarketplaces`、`skippedPlugins`、`pluginConfigs` — 仍在 JSON schema 中但不在官方设置页面上 | ✋ ON HOLD（保留在报告中 — 根据 schema 有效，自 2026-03-05 起 recurring） |

---

## [2026-03-15 01:10 PM PKT] Claude Code v2.1.76

| # | 优先级 | 类型 | 操作 | 状态 |
|---|----------|------|--------|--------|
| 1 | HIGH | 新设置项 | 添加 `effortLevel` 到模型配置 — 跨会话保持努力级别（`"low"`、`"medium"`、`"high"`）。已添加到有用命令的 `/effort` 命令并更新了努力级别操作指南 | ✅ COMPLETE（已添加到模型覆盖表，更新了操作指南，添加了 /effort 命令） |
| 2 | HIGH | 新设置项 | 添加工作树设置部分，含 `worktree.sparsePaths`（数组，稀疏检出锥模式）和 `worktree.symlinkDirectories`（数组，要去重的符号链接目录） | ✅ COMPLETE（核心配置中的新工作树设置子部分，含表和示例） |
| 3 | HIGH | 新设置项 | 添加 `feedbackSurveyRate` 到常规设置 — 会话质量调查的概率（0-1） | ✅ COMPLETE（已添加到常规设置表） |
| 4 | HIGH | 缺失环境变量 | 添加23个缺失的环境变量到表（20个真正新的 + 3个从代码块移入的） | ✅ COMPLETE（已将全部23个环境变量添加到通用环境变量表） |
| 5 | HIGH | 失效链接 | 上次运行标记的 `https://claudelog.com/configuration/` 为 ECONNREFUSED — 现已可正常加载 | ✅ COMPLETE（链接已恢复，无需操作） |
| 6 | MED | 权限语法 | 添加读写 gitignore 风格路径模式（`//path`、`~/path`、`/path`、`./path`）、词边界通配符说明和遗留 `:*` 弃用说明 | ✅ COMPLETE（已添加路径模式表、词边界说明和 `:*` 弃用） |
| 7 | MED | 描述变更 | 更新 `cleanupPeriodDays` 以添加设置为0时"钩子收到空的 `transcript_path`" | ✅ COMPLETE（已添加到描述） |
| 8 | MED | 未验证环境变量 | 将不在官方文档中的7个环境变量标记为未验证 | ✅ COMPLETE（已添加"不在官方文档中 — 未验证"标记） |
| 9 | MED | 新来源 | 添加 `https://code.claude.com/docs/en/env-vars` 和 `https://code.claude.com/docs/en/permissions` 到来源部分 | ✅ COMPLETE（已添加两个 URL） |
| 10 | MED | 示例更新 | 更新快速参考示例以包含 `effortLevel` 和 `worktree` 设置项 | ✅ COMPLETE（已添加 effortLevel 和 worktree 块到示例） |
| 11 | LOW | 可疑键名 | `sandbox.ignoreViolations`、`sandbox.network.deniedDomains` 仍不在官方文档沙盒表中 | ✋ ON HOLD（保留在报告中等待验证 — 自 2026-03-05 起 recurring） |
| 12 | LOW | 可疑键名 | `skipWebFetchPreflight`、`skippedMarketplaces`、`skippedPlugins`、`pluginConfigs` — 仍在 JSON schema 中但不在官方设置页面上 | ✋ ON HOLD（保留在报告中 — 根据 schema 有效，自 2026-03-05 起 recurring） |

---

## [2026-03-17 12:54 PM PKT] Claude Code v2.1.77

| # | 优先级 | 类型 | 操作 | 状态 |
|---|----------|------|--------|--------|
| 1 | HIGH | 新设置项 | 添加 `sandbox.filesystem.allowRead` 到沙盒设置表 — 在 `denyRead` 区域内重新允许读取访问（数组，默认为 `[]`）。在 v2.1.77 变更日志中确认 | ✅ COMPLETE（已添加到沙盒设置表，在 denyRead 行之后） |
| 2 | HIGH | 描述变更 | 更新 `CLAUDE_CODE_MAX_OUTPUT_TOKENS` 描述：Opus 4.6 默认值增加到 64k，Opus 4.6 和 Sonnet 4.6 上限增加到 128k（v2.1.77 变更日志） | ✅ COMPLETE（描述已更新，包含模型特定默认值和限制） |
| 3 | HIGH | 缺失环境变量 | 添加 `CLAUDECODE` 到通用环境变量表 — 在生成 shell 环境中设置为 `1`。在官方 /en/env-vars 页面确认 | ✅ COMPLETE（已添加到环境变量表） |
| 4 | HIGH | 缺失环境变量 | 添加 `CLAUDE_CODE_SKIP_FAST_MODE_NETWORK_ERRORS` 到通用环境变量表 — 允许在组织状态检查失败时使用快速模式。在官方 /en/env-vars 页面确认 | ✅ COMPLETE（已添加到环境变量表） |
| 5 | MED | 环境变量表 | 将 `ANTHROPIC_MODEL` 和 `ANTHROPIC_DEFAULT_HAIKU_MODEL` 从代码块移到通用环境变量表。两个都在官方 /en/env-vars 页面上确认 | ✅ COMPLETE（已在其他 ANTHROPIC_ 变量附近添加） |
| 6 | MED | 可疑键名升级 | `sandbox.network.deniedDomains` — 已连续8次 ON HOLD（自 2026-03-05 起）。不在官方文档页面或 JSON schema 中。根据规则10B：标记为"不在官方文档中 — 未验证" | ✅ COMPLETE（已在描述中添加未验证注解） |
| 7 | MED | 可疑键名升级 | `allow_remote_sessions` — 不在官方文档页面或 JSON schema 中。标记为"不在官方文档中 — 未验证" | ✅ COMPLETE（已在描述中添加未验证注解） |
| 8 | LOW | 可疑键名解决 | `sandbox.ignoreViolations` — 已连续8次 ON HOLD。在 JSON schema 中确认。注解："在 JSON schema 中，不在官方设置页面上" | ✅ COMPLETE（已在描述中添加 schema 注解） |
| 9 | LOW | 可疑键名解决 | `skipWebFetchPreflight`、`skippedMarketplaces`、`skippedPlugins`、`pluginConfigs` — 已连续8次 ON HOLD。全部在 JSON schema 中确认。注解："在 JSON schema 中，不在官方设置页面上" | ✅ COMPLETE（已添加到全部4个描述中） |
| 10 | LOW | 标题计数 | 将标题环境变量计数从"160+"更新为"100+" — 实际表中有97个环境变量 | ✅ COMPLETE（标题已更新为"100+ environment variables"，版本到 v2.1.77） |

---

## [2026-03-18 11:53 PM PKT] Claude Code v2.1.78

| # | 优先级 | 类型 | 操作 | 状态 |
|---|----------|------|--------|--------|
| 1 | HIGH | 缺失设置项 | 添加 `voiceEnabled` 到常规设置表 — 启用点击即说语音听写（boolean，由 `/voice` 写入，需要 Claude.ai 账户）。在官方设置页面上确认 | ✅ COMPLETE（已添加到常规设置表，在 feedbackSurveyRate 之前） |
| 2 | HIGH | 缺失设置项 | 添加 `filesystem.allowManagedReadPathsOnly` 到沙盒设置表 — 仅托管，仅托管的 `allowRead` 路径生效（boolean，默认为 false）。在官方设置页面上确认 | ✅ COMPLETE（已添加到沙盒设置表，在 enableWeakerNetworkIsolation 之前） |
| 3 | HIGH | 显示位置 | 将 `showTurnDuration` 和 `terminalProgressBarEnabled` 从显示设置表移至单独的"全局配置设置（~/.claude.json）"子部分。官方文档说明："将它们添加到 settings.json 会触发架构验证错误" | ✅ COMPLETE（创建了新子部分含表；从 settings.json 显示设置表和示例中移除） |
| 4 | HIGH | 默认值变更 | 修复 `MAX_MCP_OUTPUT_TOKENS` 默认值从 50000 到 25000。官方 /en/env-vars 页面确认默认值：25000 | ✅ COMPLETE（默认值已更新，添加了警告阈值说明） |
| 5 | HIGH | 缺失环境变量 | 添加 `CLAUDE_CODE_NEW_INIT`、`CLAUDE_CODE_PLUGIN_SEED_DIR`、`DISABLE_FEEDBACK_COMMAND` 到环境变量表。全部在官方 /en/env-vars 页面上确认 | ✅ COMPLETE（已将全部3个环境变量添加到表） |
| 6 | MED | 验证修复 | 移除 `allow_remote_sessions` 的"未验证"注解 — 现已在官方权限页面上确认为仅托管设置项。上次运行（v2.1.77 #7）错误地将其标记为未验证 | ✅ COMPLETE（已移除"未验证"注解） |
| 7 | MED | 环境变量重命名 | 将 `DISABLE_BUG_COMMAND` 更新为 `DISABLE_FEEDBACK_COMMAND` — 官方文档说明 `DISABLE_FEEDBACK_COMMAND` 是当前名称，`DISABLE_BUG_COMMAND` 是"旧名称" | ✅ COMPLETE（已重命名并添加别名说明） |
| 8 | MED | 描述变更 | 更新 `CLAUDE_CODE_EFFORT_LEVEL` 以包含 `max`（仅 Opus 4.6）和 `auto` 值。官方 /en/env-vars 页面确认："值：low、medium、high、max（仅 Opus 4.6）或 auto" | ✅ COMPLETE（描述已更新，包含全部值和优先级说明） |
| 9 | MED | 描述变更 | 修复 `CLAUDE_CODE_ENABLE_TASKS` 描述 — 官方说明："设置为 true 以在非交互模式（-p 标志）中启用任务跟踪。任务在交互模式下默认开启。"报告当前说明"设置为 false 以禁用" | ✅ COMPLETE（描述已修正以匹配官方文档） |
| 10 | MED | 描述变更 | 更新 `CLAUDE_CODE_DISABLE_NONESSENTIAL_TRAFFIC` 以说明："相当于设置 DISABLE_AUTOUPDATER、DISABLE_FEEDBACK_COMMAND、DISABLE_ERROR_REPORTING 和 DISABLE_TELEMETRY" | ✅ COMPLETE（描述已更新，包含等效变量列表） |
| 11 | MED | 示例更新 | 从快速参考示例中移除 `showTurnDuration` — 根据官方文档不属于 settings.json | ✅ COMPLETE（已从快速参考示例和显示与体验示例中移除） |
| 12 | LOW | 环境变量默认值 | 验证 `MCP_TIMEOUT` 默认值（报告说明 10000）— 官方文档未指定默认值 | ✅ COMPLETE（已移除未验证的默认值 — 官方文档省略了它） |

---

## [2026-03-19 12:38 PM PKT] Claude Code v2.1.79

| # | 优先级 | 类型 | 操作 | 状态 |
|---|----------|------|--------|--------|
| 1 | HIGH | 缺失环境变量 | 添加 `ANTHROPIC_CUSTOM_MODEL_OPTION`、`ANTHROPIC_CUSTOM_MODEL_OPTION_NAME`、`ANTHROPIC_CUSTOM_MODEL_OPTION_DESCRIPTION` 到通用环境变量表 — 用于在 `/model` 选择器中添加自定义条目的模型配置变量。在官方 /en/env-vars 页面上确认 | ✅ COMPLETE（在 ANTHROPIC_BASE_URL 之后添加了3个环境变量） |
| 2 | HIGH | 描述变更 | 更新 `CLAUDE_CODE_PLUGIN_SEED_DIR` 从单数到复数："指向一个或多个只读插件种子目录的路径，Unix 上用 `:` 分隔，Windows 上用 `;` 分隔"。在 v2.1.79 变更日志中更改。在官方 /en/env-vars 页面上确认 | ✅ COMPLETE（描述已更新以支持多目录） |
| 3 | HIGH | 沙盒路径前缀 | 修复 sandbox.filesystem 路径前缀文档：`/` = 绝对路径（标准 Unix），`./` = 项目相对路径，`//` = 遗留形式仍可用。报告当前显示的约定相反。官方文档明确说明："此语法与读写权限规则不同" | ✅ COMPLETE（已用正确的前缀约定更新全部4个 sandbox.filesystem 条目，添加了读写权限规则交叉引用说明，添加了跨作用域合并详情） |
| 4 | MED | 描述变更 | 扩展 `CLAUDE_CODE_AUTO_COMPACT_WINDOW` 描述 — 当前"自动压缩窗口行为配置"过于简略。官方文档描述：token 容量、默认值（200K 标准 / 1M 扩展）、与 `CLAUDE_AUTOCOMPACT_PCT_OVERRIDE` 的交互、状态行解耦 | ✅ COMPLETE（扩展描述包含 token 容量、模型默认值、AUTOCOMPACT_PCT 交互和状态行解耦） |

---

## [2026-03-20 08:41 AM PKT] Claude Code v2.1.80

| # | 优先级 | 类型 | 操作 | 状态 |
|---|----------|------|--------|--------|
| 1 | HIGH | 新设置项 | 添加 `channelsEnabled` 到 MCP 设置表 — 仅托管 boolean，控制 Team 和 Enterprise 用户的通道消息传递。在官方设置页面上确认 | ✅ COMPLETE（已添加到 MCP 设置表，在 allowManagedMcpServersOnly 之后） |
| 2 | MED | 版本徽章 | 将报告版本从 v2.1.79 更新到 v2.1.80 | ✅ COMPLETE（徽章和标题已更新） |

---

## [2026-03-21 09:17 PM PKT] Claude Code v2.1.81

| # | 优先级 | 类型 | 操作 | 状态 |
|---|----------|------|--------|--------|
| 1 | HIGH | 缺失设置项（~/.claude.json） | 添加 `autoConnectIde`（boolean，默认为 `false`）和 `autoInstallIdeExtension`（boolean，默认为 `true`）到全局配置设置表。在官方设置页面的"全局配置设置"下确认 | ✅ COMPLETE（已将两个键添加到 ~/.claude.json 表，在 showTurnDuration 之前） |
| 2 | HIGH | 错误设置项 | `allow_remote_sessions` 在权限键表中列为仅托管 boolean，但官方权限页面说明："远程控制和 Web 会话的访问不受托管设置键控制。"标记为未验证或移除 | ✅ COMPLETE（重新添加未验证注解，含官方文档引用和管理员 UI 链接） |
| 3 | MED | 版本更新 | 将报告版本徽章从 v2.1.80 更新到 v2.1.81 | ✅ COMPLETE（徽章、标题版本和标题文本已更新） |
| 4 | MED | 新设置项 | 添加 `showClearContextOnPlanAccept` — 在 v2.1.81 变更日志中确认。设置为 `true` 时，恢复计划接受时的"清除上下文"选项（默认隐藏）。尚不在官方设置页面上 — 可能是一个 `~/.claude.json` 键 | ✅ COMPLETE（已添加到全局配置设置表，含变更日志来源说明） |
| 5 | MED | 插件文档 | 在插件设置部分记录 `source: 'settings'` 作为市场来源类型。官方设置页面将其列为 `extraKnownMarketplaces` 的7个来源类型之一 | ✅ COMPLETE（已添加全部7个来源类型列表，内联市场示例） |
| 6 | MED | 状态行字段 | 添加 `rate_limits` 字段组到状态行输入字段表 — 含 `five_hour.used_percentage`、`five_hour.resets_at`、`seven_day.used_percentage`、`seven_day.resets_at`。在 v2.1.80 添加 | ✅ COMPLETE（已将4个 rate_limits 字段添加到状态行输入字段表） |

---

## [2026-03-23 10:02 PM PKT] Claude Code v2.1.81

| # | 优先级 | 类型 | 操作 | 状态 |
|---|----------|------|--------|--------|
| 1 | HIGH | 缺失设置项（~/.claude.json） | 添加 `editorMode`（字符串，默认为 `"normal"`，值：`"normal"` 或 `"vim"`）到全局配置设置表。运行 `/vim` 时自动写入。在官方设置页面上确认 | ✅ COMPLETE（已添加到全局配置设置表，在 autoInstallIdeExtension 之后） |
| 2 | HIGH | 文件作用域修复 | 将 `showClearContextOnPlanAccept` 从全局配置设置（~/.claude.json）移到常规设置（settings.json）。官方文档现在将其列在主可用设置表中，不在全局配置表中。移除过时的"尚不在官方设置页面上"注解 | ✅ COMPLETE（已移到常规设置表，在 feedbackSurveyRate 之前，移除过时注解） |
| 3 | MED | 描述变更 | 修复 `terminalProgressBarEnabled` 支持的终端从"Windows Terminal、iTerm2"到"ConEmu、Ghostty 1.2.0+ 和 iTerm2 3.6.6+"（根据官方文档） | ✅ COMPLETE（终端列表已更新） |
| 4 | MED | 描述变更 | 在 `availableModels` 描述中添加"配置工具" — 官方文档说明："通过 `/model`、`--model`、配置工具或 `ANTHROPIC_MODEL`"。报告当前遗漏了"配置工具" | ✅ COMPLETE（已在描述中添加"配置工具"） |

---

## [2026-03-25 08:16 PM PKT] Claude Code v2.1.83

| # | 优先级 | 类型 | 操作 | 状态 |
|---|----------|------|--------|--------|
| 1 | HIGH | 新设置项 | 添加 `autoMode` 到权限部分 — 含 `environment`、`allow`、`soft_deny` 数组的对象，用于配置自动模式分类器。不从共享项目设置（`.claude/settings.json`）读取。可在用户、本地和托管设置中设置。在官方设置和权限页面上确认 | ✅ COMPLETE（已添加到权限键表，含完整描述、作用域限制和 `claude auto-mode defaults` 说明） |
| 2 | HIGH | 新设置项 | 添加 `disableAutoMode` 到权限部分 — 字符串，设置为 `"disable"` 以防止自动模式激活。从 Shift+Tab 循环中移除 `auto`。可在任何设置级别设置，在托管设置中最有用。在官方设置和权限页面上确认 | ✅ COMPLETE（已添加到权限键表，在 `autoMode` 之后） |
| 3 | HIGH | 新权限模式 | 添加 `auto` 到权限模式表 — 后台分类器替代手动提示。研究预览。需要 Team 计划 + Sonnet/Opus 4.6。在官方权限模式页面上确认 | ✅ COMPLETE（已添加到权限模式表，含分类器详情和回退行为） |
| 4 | HIGH | 新设置项 | 添加 `sandbox.failIfUnavailable` 到沙盒设置表 — boolean，默认为 `false`，当启用沙盒但无法启动时退出并报错，而不是无沙盒运行。在 v2.1.83 变更日志中确认 | ✅ COMPLETE（已添加到沙盒设置表，在 `sandbox.enabled` 之后） |
| 5 | HIGH | 新设置项 | 添加 `disableDeepLinkRegistration` 到常规设置表 — boolean，防止 `claude-cli://` 协议处理器注册。在 v2.1.83 变更日志中确认 | ✅ COMPLETE（已添加到常规设置表，在 `feedbackSurveyRate` 之前） |
| 6 | HIGH | 缺失环境变量 | 添加 `CLAUDE_CODE_SUBPROCESS_ENV_SCRUB` 到通用环境变量表 — 设置为 `1` 以从子进程环境（Bash 工具、钩子、MCP stdio 服务器）中剥离 Anthropic 和云提供商凭据。在 v2.1.83 变更日志中确认 | ✅ COMPLETE（已添加到环境变量表，在 `CLAUDE_CODE_SUBAGENT_MODEL` 之后） |
| 7 | HIGH | 设置层级 | 添加 `managed-settings.d/` drop-in 目录到托管设置部分 — 与 `managed-settings.json` 并列的独立策略片段，按字母顺序合并。在 v2.1.83 变更日志中确认 | ✅ COMPLETE（已添加为托管设置传递方法的要点） |
| 8 | HIGH | 失效链接 | 修复来源中的 `https://claudelog.com/configuration/` — 返回 403 Forbidden。移除或替换为有效来源 | ✅ COMPLETE（替换为 `https://claudelog.com/claude-code-changelog/`，已验证可用） |
| 9 | MED | 版本徽章 | 将报告版本从 v2.1.81 更新到 v2.1.83 | ✅ COMPLETE（徽章和标题已在第2.6阶段更新） |
| 10 | MED | 示例更新 | 在快速参考示例中添加 `autoMode` 以演示自动模式分类器配置 | ✅ COMPLETE（已在 `permissions` 块之前添加含 `environment` 数组的 `autoMode` 块） |
| 11 | MED | 路径变更 | 修复 Windows 注册表路径从 `Software\Anthropic\ClaudeCode` 到 `SOFTWARE\Policies\ClaudeCode`（HKLM 和 HKCU）。官方文档已更新使用 `Policies` 子键 | ✅ COMPLETE（已更新为 `HKLM\SOFTWARE\Policies\ClaudeCode` 和 `HKCU\SOFTWARE\Policies\ClaudeCode`，含优先级说明） |
| 12 | LOW | 缺失别名 | 在模型别名表中添加 `opus[1m]` — 带 1M 上下文的 Opus 4.6，自 v2.1.75 起在 Max/Team/Enterprise 上默认可用 | ✅ COMPLETE（已在 `sonnet[1m]` 之后添加到模型别名表） |

---

## [2026-03-26 01:04 PM PKT] Claude Code v2.1.84

| # | 优先级 | 类型 | 操作 | 状态 |
|---|----------|------|--------|--------|
| 1 | HIGH | 新设置项 | 添加 `defaultShell` 到常规设置 — 字符串，默认为 `"bash"`，接受 `"bash"` 或 `"powershell"`。在 Windows 上通过 PowerShell 路由交互式 `!` 命令。需要 `CLAUDE_CODE_USE_POWERSHELL_TOOL=1`。在官方设置页面上确认 | ✅ COMPLETE（已添加到常规设置表，在 teammateMode 之后） |
| 2 | HIGH | 新设置项 | 添加 `allowedChannelPlugins` 到 MCP 设置 — 数组，仅托管。可以推送消息的通道插件允许列表。设置时替换默认的 Anthropic 允许列表。需要 `channelsEnabled: true`。在官方设置页面上确认 | ✅ COMPLETE（已添加到 MCP 设置表，在 channelsEnabled 之后） |
| 3 | HIGH | 新设置项 | 添加 `useAutoModeDuringPlan` 到权限键 — boolean，默认为 `true`。自动模式可用时，计划模式使用自动模式语义。不从共享项目设置读取。在官方设置页面上确认 | ✅ COMPLETE（已添加到权限键表，在 disableAutoMode 之后） |
| 4 | HIGH | 缺失环境变量 | 添加9个模型定制环境变量：`ANTHROPIC_DEFAULT_{OPUS,SONNET,HAIKU}_MODEL_{NAME,DESCRIPTION,SUPPORTED_CAPABILITIES}`，用于 Bedrock/Vertex/Foundry 上的 `/model` 选择器定制。在官方 /en/env-vars 页面上确认 | ✅ COMPLETE（在每个基础模型变量后添加了3个变量：Haiku、Opus、Sonnet） |
| 5 | HIGH | 缺失环境变量 | 添加 `CLAUDE_CODE_DISABLE_NONSTREAMING_FALLBACK` — 禁用流式失败时的非流式回退。防止通过代理重复执行工具。在官方 /en/env-vars 页面上确认（在 v2.1.83 添加，但上次运行遗漏） | ✅ COMPLETE（已在 CLAUDE_CODE_DISABLE_FAST_MODE 之后添加） |
| 6 | HIGH | 缺失环境变量 | 添加 `CLAUDE_CODE_USE_POWERSHELL_TOOL` — 在 Windows 上启用 PowerShell 工具（opt-in 预览）。仅原生 Windows，WSL 不可用。在官方 /en/env-vars 页面上确认 | ✅ COMPLETE（已在 CLAUDE_CODE_USE_FOUNDRY 之后添加） |
| 7 | HIGH | 失效链接 | 修复来源中的 `https://claudelog.com/claude-code-changelog/` — 返回 403 Forbidden。替换为官方 GitHub 变更日志 URL | ✅ COMPLETE（替换为 github.com/anthropics/claude-code/blob/main/CHANGELOG.md） |
| 8 | MED | 设置层级 | 更新托管层级优先级："基于文件的（`managed-settings.d/*.json` + `managed-settings.json`）"并添加"跨层级"限定符。根据官方文档添加同层级合并说明 | ✅ COMPLETE（已用基于文件的层级和跨层级限定符更新优先级描述） |
| 9 | MED | 设置层级 | 扩展 drop-in 目录合并语义：systemd 约定、标量覆盖、数组连接+去重、深度合并、隐藏文件排除、数字前缀提示。根据官方设置页面 | ✅ COMPLETE（已用完整的 systemd 约定细节和数字前缀提示扩展） |
| 10 | MED | 注解 | 根据规则1F 反向完整性检查，为 `disableDeepLinkRegistration` 添加"在变更日志中，不在官方设置页面上"注解 | ✅ COMPLETE（已在描述中添加注解） |
| 11 | MED | 示例更新 | 在快速参考示例中添加 `defaultShell` 以演示 PowerShell 配置 | ✅ COMPLETE（已在示例中添加 "defaultShell": "bash"） |

---

## [2026-03-27 06:32 PM PKT] Claude Code v2.1.85

| # | 优先级 | 类型 | 操作 | 状态 |
|---|----------|------|--------|--------|
| 1 | HIGH | 缺失环境变量 | 添加 `CLAUDE_STREAM_IDLE_TIMEOUT_MS` 到通用环境变量表 — 流式空闲看门狗关闭停滞连接前的超时毫秒数（默认值：90000）。在官方 /en/env-vars 页面上确认。在 v2.1.84 添加但上次运行遗漏 | ✅ COMPLETE（已在 CLAUDE_CODE_OTEL_HEADERS_HELPER_DEBOUNCE_MS 之后添加到环境变量表） |
| 2 | HIGH | 版本更新 | 将报告版本徽章从 v2.1.84 更新到 v2.1.85 | ✅ COMPLETE（徽章、标题版本和标题文本已在第2.6阶段更新） |
| 3 | MED | 新环境变量 | 添加 `OTEL_LOG_TOOL_DETAILS` 到环境变量表 — 控制 OpenTelemetry 事件中的 `tool_parameters`。仅 v2.1.85 变更日志（尚不在官方 env-vars 页面上）。添加时带变更日志来源注解 | ✅ COMPLETE（已添加，含"在 v2.1.85 变更日志中，尚未在官方 env-vars 页面上"注解） |
| 4 | MED | 新环境变量（所有权） | 为 `CLAUDE_CODE_MCP_SERVER_NAME` 和 `CLAUDE_CODE_MCP_SERVER_URL` 决定所有权 — 传递给 MCP `headersHelper` 脚本的环境变量（v2.1.85 变更日志）。可能属于钩子仓库而非设置报告 | ✅ COMPLETE（已添加到设置报告，含变更日志注解 — 这些是通过 `env` 键可配置的，不是仅钩子的） |

---

## [2026-03-28 06:10 PM PKT] Claude Code v2.1.86

| # | 优先级 | 类型 | 操作 | 状态 |
|---|----------|------|--------|--------|
| 1 | HIGH | 文件作用域 | 将 `teammateMode` 从常规设置（settings.json）移到全局配置设置（~/.claude.json）。官方设置页面将其列在"全局配置设置"下 — 添加到 settings.json 会触发架构验证错误（规则1H）。与 v2.1.78 `showTurnDuration` 修复相同的模式 | ✅ COMPLETE（已从常规设置表移除，添加到全局配置设置表，在 terminalProgressBarEnabled 之后，含代理团队文档链接） |
| 2 | HIGH | 类型 + 注解 | 修复 `disableDeepLinkRegistration`：类型从 `boolean` 改为 `string`（值：`"disable"`），更新描述以匹配官方文档，移除过时的"（在变更日志中，不在官方设置页面上）"注解。现已在官方设置页面上确认（第169行） | ✅ COMPLETE（类型已改为 string，描述已更新以匹配官方文档，变更日志注解已移除） |
| 3 | HIGH | 版本更新 | 将报告版本徽章从 v2.1.85 更新到 v2.1.86 | ✅ COMPLETE（徽章和标题已在第2.6阶段更新） |

---

## [2026-03-31 07:02 PM PKT] Claude Code v2.1.88

| # | 优先级 | 类型 | 操作 | 状态 |
|---|----------|------|--------|--------|
| 1 | HIGH | 缺失环境变量 | 添加 `CLAUDE_CODE_NO_FLICKER` 到通用环境变量表 — 启用无闪烁替代屏幕渲染（v2.1.88）。在官方 /en/env-vars 页面上确认 | ✅ COMPLETE（已在 CLAUDE_CODE_DISABLE_TERMINAL_TITLE 之后添加） |
| 2 | HIGH | 缺失环境变量 | 添加 `CLAUDE_CODE_SCROLL_SPEED` 和 `CLAUDE_CODE_DISABLE_MOUSE` 到通用环境变量表 — 全屏 UI 控制。在官方 /en/env-vars 页面上确认 | ✅ COMPLETE（已在 CLAUDE_CODE_NO_FLICKER 之后添加） |
| 3 | HIGH | 版本更新 | 将报告版本徽章从 v2.1.86 更新到 v2.1.88 | ✅ COMPLETE（徽章、标题版本和标题文本已在第2.6阶段更新） |
| 4 | HIGH | 失效链接 | 修复来源中的 `https://www.eesel.ai/blog/settings-json-claude-code` — 返回仅 CSS 内容，没有可读博客文章 | ✅ COMPLETE（已从来源部分移除失效链接） |
| 5 | MED | 设置层级 | 添加 `managed-mcp.json` 到基于文件的托管传递方法 — 官方设置页面将其与 `managed-settings.json` 并列列出用于 MCP 服务器配置 | ✅ COMPLETE（已在层级设置的基于文件的传递方法中添加） |
| 6 | MED | 插件来源类型 | 为 `url`、`npm`、`file` 市场来源类型添加"不在官方文档中 — 未验证"注解（仅 `github`、`git`、`directory`、`hostPattern`、`settings` 已确认） | ✅ COMPLETE（已为全部3个来源类型添加未验证注解） |
| 7 | LOW | 标题计数 | 将标题从"60+ 设置项"更新以匹配添加任何内容后的实际表计数 | ❌ INVALID（计数是准确的 — 60+ 设置项和 125 个环境变量，都在所述范围内） |

---

## [2026-04-01 12:32 PM PKT] Claude Code v2.1.89

| # | 优先级 | 类型 | 操作 | 状态 |
|---|----------|------|--------|--------|
| 1 | HIGH | 缺失设置项 | 添加 `skipDangerousModePermissionPrompt` 到权限键表 — boolean，跳过 bypass 模式确认提示。在项目设置中被忽略。在官方设置页面上确认 | ✅ COMPLETE（已在权限键表中的 disableBypassPermissionsMode 之后添加） |
| 2 | HIGH | 新设置项 | 添加 `showThinkingSummaries` 到常规设置 — boolean，默认为 `false`。思维摘要不再默认生成；设置为 `true` 以恢复。v2.1.89 变更日志 — 尚不在官方设置页面上 | ✅ COMPLETE（已在 feedbackSurveyRate 之前添加，含变更日志注解） |
| 3 | HIGH | 行为变更 | 更新 `cleanupPeriodDays` 描述 — v2.1.89 变更日志说明 `0` 现在被拒绝并抛出验证错误。矛盾：官方设置页面仍描述 `0` 为有效。标记给用户 | ✅ COMPLETE（已在描述中添加变更日志和文档页面之间的矛盾说明） |
| 4 | HIGH | 缺失环境变量 | 添加约46个在官方 /en/env-vars 页面上确认的缺失环境变量：`ANTHROPIC_BEDROCK_BASE_URL`、`ANTHROPIC_VERTEX_BASE_URL`、`ANTHROPIC_BETAS`、`ANTHROPIC_VERTEX_PROJECT_ID`、`CLAUDE_CODE_DISABLE_THINKING`、`DISABLE_INTERLEAVED_THINKING`、`ENABLE_PROMPT_CACHING_1H_BEDROCK`、`DISABLE_AUTO_COMPACT`、`DISABLE_COMPACT`、`CLAUDE_CODE_DISABLE_FILE_CHECKPOINTING`、`CLAUDE_CODE_DISABLE_ATTACHMENTS`、`CLAUDE_CODE_DISABLE_CLAUDE_MDS`、`CLAUDE_CODE_GLOB_HIDDEN`、`CLAUDE_CODE_GLOB_NO_IGNORE`、`CLAUDE_CODE_GLOB_TIMEOUT_SECONDS`、`CLAUDE_CODE_DISABLE_OFFICIAL_MARKETPLACE_AUTOINSTALL`、`CLAUDE_CODE_SYNC_PLUGIN_INSTALL`、`CLAUDE_CODE_SYNC_PLUGIN_INSTALL_TIMEOUT_MS`、`CLAUDE_CODE_AUTO_CONNECT_IDE`、`CLAUDE_CODE_IDE_HOST_OVERRIDE`、`CLAUDE_CODE_IDE_SKIP_VALID_CHECK`、`CLAUDE_CODE_MAX_RETRIES`、`API_TIMEOUT_MS`、`CLAUDE_CODE_OTEL_FLUSH_TIMEOUT_MS`、`CLAUDE_CODE_OTEL_SHUTDOWN_TIMEOUT_MS`、`CLAUDE_ENABLE_STREAM_WATCHDOG`、`CLAUDE_CODE_ENABLE_FINE_GRAINED_TOOL_STREAMING`、`CLAUDE_CODE_DEBUG_LOGS_DIR`、`CLAUDE_CODE_DEBUG_LOG_LEVEL`、`CLAUDE_CODE_ACCESSIBILITY`、`CLAUDE_CODE_SYNTAX_HIGHLIGHT`、`CLAUDE_CODE_RESUME_INTERRUPTED_TURN`、`CLAUDE_CODE_MAX_TOOL_USE_CONCURRENCY`、`CLAUDE_CODE_DISABLE_LEGACY_MODEL_REMAP`、`FALLBACK_FOR_ALL_PRIMARY_MODELS`、`CLAUDE_CODE_GIT_BASH_PATH`、`CLAUDE_AUTO_BACKGROUND_TASKS`、`CLAUDE_AGENT_SDK_DISABLE_BUILTIN_AGENTS`、`CLAUDE_AGENT_SDK_MCP_NO_PREFIX`、`DISABLE_DOCTOR_COMMAND`、`DISABLE_LOGIN_COMMAND`、`DISABLE_LOGOUT_COMMAND`、`DISABLE_UPGRADE_COMMAND`、`DISABLE_EXTRA_USAGE_COMMAND`、`DISABLE_INSTALL_GITHUB_APP_COMMAND`、`CLAUDE_CODE_PLUGIN_CACHE_DIR`、`CLAUDE_CODE_SIMPLE` | ✅ COMPLETE（已将全部46个环境变量添加到表中相关变量附近） |
| 5 | HIGH | 版本更新 | 将报告版本徽章从 v2.1.88 更新到 v2.1.89 | ✅ COMPLETE（徽章和标题已在第2.6阶段更新） |
| 6 | MED | 新环境变量 | 添加 `MCP_CONNECTION_NONBLOCKING` 到环境变量表 — 在 `-p` 模式中设置为 `true` 以跳过 MCP 连接等待。仅 v2.1.89 变更日志，尚未在官方 /en/env-vars 页面上 | ✅ COMPLETE（已在 CLAUDE_AGENT_SDK_MCP_NO_PREFIX 之后添加，含变更日志注解） |
| 7 | MED | 所有权边界 | `CLAUDE_CODE_SIMPLE` 在 CLI 启动标志文件中作为仅启动标志，但官方 /en/env-vars 页面将其列为可配置。协调所有权 | ✅ COMPLETE（已添加到设置报告环境表；已更新 CLI 文件以交叉引用设置报告） |
| 8 | MED | 示例更新 | 更新快速参考示例以包含 `showThinkingSummaries`（如果已添加） | ✅ COMPLETE（已在示例中添加 showThinkingSummaries: true） |

---

## [2026-04-02 09:24 PM PKT] Claude Code v2.1.90

| # | 优先级 | 类型 | 操作 | 状态 |
|---|----------|------|--------|--------|
| 1 | HIGH | 类型 + 描述变更 | 修复 `forceLoginOrgUUID`：类型从 `string` 到 `string \| string[]`。扩展描述以包含数组行为（任何列出的 org 无需预选即可接受）、托管设置 enforcement（如果账户不在列出的 org 中则登录失败）和空数组 fail-closed 行为 | ✅ COMPLETE（类型已更新为 string \| array，描述已扩展含数组行为、托管 enforcement、fail-closed 语义，示例已更新） |
| 2 | HIGH | 缺失环境变量 | 添加 `CLAUDE_CODE_OAUTH_TOKEN`、`CLAUDE_CODE_OAUTH_REFRESH_TOKEN`、`CLAUDE_CODE_OAUTH_SCOPES` 到通用环境变量表。全部在官方 /en/env-vars 页面上确认 | ✅ COMPLETE（已在 ANTHROPIC_AUTH_TOKEN 之后添加3个 OAuth 环境变量） |
| 3 | HIGH | 描述 + 注解变更 | 更新 `showThinkingSummaries`：移除"（在 v2.1.89 变更日志中，尚未在官方设置页面上）"注解 — 现已在官方设置页面上确认。更新描述以匹配官方："未设置或为 false（交互模式下默认）时，思维块由 API 编辑并显示为折叠存根。编辑只改变您看到的内容，不改变模型生成的内容" | ✅ COMPLETE（注解已移除，描述已更新以匹配官方文档） |
| 4 | HIGH | 沙盒交叉合并 | 更新 `sandbox.filesystem.allowWrite` 描述以添加"也与 `Edit(...)` 允许权限规则的路径合并"。更新 `denyWrite` 以添加"也与 `Edit(...)` 拒绝权限规则的路径合并"。更新 `denyRead` 以添加"也与 `Read(...)` 拒绝权限规则的路径合并"。在官方设置页面上确认 | ✅ COMPLETE（已为全部3个文件系统条目添加交叉合并行为） |
| 5 | HIGH | 描述变更 | 简化 `cleanupPeriodDays` 描述：移除矛盾说明，与官方文档保持一致，官方文档现在说明"最小值1，设置为0会被拒绝并抛出验证错误"。官方页面上不再记录旧行为 | ✅ COMPLETE（矛盾说明已移除，描述已与官方文档对齐，添加了 --no-session-persistence 替代方案） |
| 6 | HIGH | 版本更新 | 将报告版本徽章从 v2.1.89 更新到 v2.1.90 | ✅ COMPLETE（徽章、标题版本和标题文本已更新） |
| 7 | MED | 新环境变量 | 添加 `CLAUDE_CODE_PLUGIN_KEEP_MARKETPLACE_ON_FAILURE` 到环境变量表 — 在 git pull 失败时保留市场缓存（v2.1.90 变更日志，尚未在官方 /en/env-vars 页面上） | ✅ COMPLETE（已在 CLAUDE_CODE_SYNC_PLUGIN_INSTALL_TIMEOUT_MS 之后添加，含变更日志注解） |
| 8 | MED | 钩子重定向计数 | 将重定向文本从"全部19个钩子事件"更新为"全部25个钩子事件"（根据官方钩子页面计数） | ✅ COMPLETE（已在钩子重定向部分更新计数） |
| 9 | MED | 所有权边界 | `CLAUDE_CODE_TMPDIR` 在官方 /en/env-vars 页面上列为可通过 `env` 键配置，但 CLI 启动标志报告显示为仅启动。协调所有权 | ✅ COMPLETE（已添加到设置报告环境表；已更新 CLI 标志文件以交叉引用设置报告） |

---

## [2026-04-03 08:44 PM PKT] Claude Code v2.1.91

| # | 优先级 | 类型 | 操作 | 状态 |
|---|----------|------|--------|--------|
| 1 | HIGH | 新设置项 | 添加 `disableSkillShellExecution` 到常规设置表 — boolean，禁用技能、自定义斜杠命令和插件命令中的内联 shell 执行。在 v2.1.91 变更日志中确认。尚不在官方设置页面上或 JSON schema 中 | ✅ COMPLETE（已在 showThinkingSummaries 之后添加，含变更日志注解） |
| 2 | HIGH | 版本更新 | 将报告版本徽章从 v2.1.90 更新到 v2.1.91 | ✅ COMPLETE（徽章和标题已在第2.6阶段更新） |

---

## [2026-04-04 10:48 PM PKT] Claude Code v2.1.92

| # | 优先级 | 类型 | 操作 | 状态 |
|---|----------|------|--------|--------|
| 1 | HIGH | 新设置项 | 添加 `forceRemoteSettingsRefresh` 到常规设置 — boolean，仅托管，阻止 CLI 启动直到远程托管设置被最新获取（fail-closed）。在官方设置页面上确认 | ✅ COMPLETE（已添加到常规设置表，在 feedbackSurveyRate 之前） |
| 2 | HIGH | 缺失环境变量 | 添加 `CLAUDE_REMOTE_CONTROL_SESSION_NAME_PREFIX` 到通用环境变量表 — 自动生成的远程控制会话名称的前缀，默认为机器主机名。在官方 /en/env-vars 页面上确认 | ✅ COMPLETE（已在 CLAUDE_CODE_ENABLE_TELEMETRY 之前添加） |
| 3 | MED | 描述变更 | 更新 `disableSkillShellExecution` — 移除"（在 v2.1.91 变更日志中，尚未在官方设置页面上）"注解。现已在官方设置页面上确认并扩展了描述 | ✅ COMPLETE（注解已移除，描述已根据官方文档扩展） |
| 4 | MED | 描述变更 | 移除市场来源类型 `url`、`npm` 和 `file` 的"不在官方文档中 — 未验证"标签。官方设置页面现在记录了全部8个来源类型 | ✅ COMPLETE（未验证标签已移除 — 自 2026-03-31 起 recurring，现已解决） |
| 5 | MED | 描述变更 | 丰富 `cleanupPeriodDays` — 添加"还控制启动时自动删除孤立子代理工作树的年龄截止"（根据官方设置页面） | ✅ COMPLETE（已添加工作树清理详情） |
| 6 | MED | 描述变更 | 丰富 `disableDeepLinkRegistration` — 添加通过 `%0A` 的多行提示支持（根据官方设置页面） | ✅ COMPLETE（已添加多行提示详情） |
| 7 | MED | 描述变更 | 丰富 `includeGitInstructions` — 更新以包含 git 状态快照和环境变量优先级（根据官方设置页面） | ✅ COMPLETE（描述已扩展含 git 状态快照和 CLAUDE_CODE_DISABLE_GIT_INSTRUCTIONS 优先级） |
| 8 | MED | 描述变更 | 丰富 `language` — 添加"还设置语音听写语言"（根据官方设置页面） | ✅ COMPLETE（已添加语音听写详情） |
| 9 | MED | 描述变更 | 丰富 `allowUnsandboxedCommands` — 添加企业策略详情（根据官方设置页面） | ✅ COMPLETE（已扩展含 fail-closed 行为和企业用例） |

---

## [2026-04-08 09:51 PM PKT] Claude Code v2.1.96

| # | 优先级 | 类型 | 操作 | 状态 |
|---|----------|------|--------|--------|
| 1 | HIGH | 缺失环境变量 | 添加 `CLAUDE_CODE_USE_MANTLE`、`ANTHROPIC_BEDROCK_MANTLE_BASE_URL`、`CLAUDE_CODE_SKIP_MANTLE_AUTH` 到通用环境变量表 — Bedrock Mantle 端点支持（v2.1.94）。全部在官方 /en/env-vars 页面上确认 | ✅ COMPLETE（已在相关云提供商变量附近添加） |
| 2 | HIGH | 默认值变更 | 更新努力级别部分 — API 密钥、Bedrock/Vertex/Foundry、Team 和 Enterprise 用户的默认值从 Medium 改为 High（v2.1.94）。更新表默认值标记和历史说明 | ✅ COMPLETE（表已更新 High 为默认值，历史说明已扩展含 v2.1.94 变更） |
| 3 | HIGH | 版本更新 | 将报告版本徽章从 v2.1.92 更新到 v2.1.96 | ✅ COMPLETE（徽章、标题版本和标题文本已在第2.6阶段更新） |
| 4 | MED | 过时注解 | 移除 `CLAUDE_CODE_PLUGIN_KEEP_MARKETPLACE_ON_FAILURE` 的"（在 v2.1.90 变更日志中，尚未在官方 env-vars 页面上）" — 现已在官方 /en/env-vars 页面上确认。更新描述以匹配官方措辞 | ✅ COMPLETE（注解已移除，描述已根据官方文档更新） |
| 5 | MED | 描述变更 | 更新 `CLAUDE_CODE_GLOB_HIDDEN` 描述以匹配官方："设置为 `false` 以从 Glob 结果中排除点文件。默认包含。不影响 `@` 文件自动完成、`ls`、Grep 或 Read" | ✅ COMPLETE（描述已根据官方 env-vars 页面重写） |

---

## [2026-04-09 11:39 PM PKT] Claude Code v2.1.97

| # | 优先级 | 类型 | 操作 | 状态 |
|---|----------|------|--------|--------|
| 1 | HIGH | 新设置项 | 添加 `sandbox.network.allowMachLookup` 到沙盒设置表 — 数组，仅 macOS，带尾部 `*` 通配符支持的 XPC/Mach 服务名称。在官方设置页面上确认 | ✅ COMPLETE（已在 allowManagedDomainsOnly 之后的沙盒网络子键中添加） |
| 2 | HIGH | 显示与体验 | 添加 `refreshInterval` 字段到状态行配置部分 — 可选，每 N 秒重新运行命令，最小值1（v2.1.97）。在官方状态行文档上确认 | ✅ COMPLETE（已添加到配置表，含 `padding` 字段，更新了 JSON 示例） |
| 3 | HIGH | 显示与体验 | 将状态行输入字段表从9个扩展到30+个字段以匹配官方状态行文档。添加 `model.*`、`workspace.*`、`cost.*`、`session_id`、`session_name`、`transcript_path`、`version`、`output_style.name`、`vim.mode`、`agent.name`、`worktree.*` 字段 | ✅ COMPLETE（根据官方状态行文档从9个扩展到30个字段） |
| 4 | HIGH | 版本更新 | 将报告版本徽章从 v2.1.96 更新到 v2.1.97 | ✅ COMPLETE（徽章和标题已在第2.6阶段更新） |
| 5 | MED | 字段命名 | 修复状态行输入字段表中的 `current_usage` → `context_window.current_usage` | ✅ COMPLETE（已用完整路径重命名并扩展描述） |
| 6 | MED | 所有权边界 | 添加 `CCR_FORCE_BUNDLE` 到 `claude-cli-startup-flags.md` — 用于 `claude --remote` 捆绑的仅启动变量。在官方 /en/env-vars 页面上但不在任一文件中 | ✅ COMPLETE（已添加到 CLI 启动标志环境变量表） |
| 7 | MED | 描述变更 | 更新 `CLAUDE_CODE_GLOB_NO_IGNORE` 描述以匹配官方："设置为 `false` 以使 Glob 工具遵守 `.gitignore` 模式。默认情况下，Glob 返回所有匹配文件包括 gitignored 的文件。不影响 `@` 文件自动完成" | ✅ COMPLETE（描述已根据官方 env-vars 页面重写） |
| 8 | MED | 描述变更 | 更新 `editorMode` 描述 — 移除过时的 `/vim` 引用（v2.1.94 中移除），将配置标签从"键绑定模式"改为"编辑器模式"（根据官方文档） | ✅ COMPLETE（已移除 /vim 引用，配置标签已更新） |
