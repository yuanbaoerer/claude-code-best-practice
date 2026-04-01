# Claude Code 设置参考

![最后更新](https://img.shields.io/badge/最后更新-Mar%2028%2C%202026%206%3A10%20PM%20PKT-white?style=flat&labelColor=555) ![版本](https://img.shields.io/badge/Claude_Code-v2.1.86-blue?style=flat&labelColor=555)

Claude Code `settings.json` 文件中所有可用配置选项的综合指南。截至 v2.1.86，Claude Code 暴露 **60+ 设置** 和 **100+ 环境变量**（使用 `settings.json` 中的 `"env"` 字段避免包装脚本）。

<table width="100%">
<tr>
<td><a href="../">← 返回 Claude Code 最佳实践</a></td>
<td align="right"><img src="../!/claude-jumping.svg" alt="Claude" width="60" /></td>
</tr>
</table>

## 目录

1. [设置层级](#设置层级)
2. [核心配置](#核心配置)
3. [权限](#权限)
4. [钩子](#钩子)
5. [MCP 服务器](#mcp-服务器)
6. [沙箱](#沙箱)
7. [插件](#插件)
8. [模型配置](#模型配置)
9. [显示与 UX](#显示与-ux)
10. [AWS 与云凭证](#aws-与云凭证)
11. [环境变量](#环境变量-via-env)
12. [实用命令](#实用命令)

---

## 设置层级

设置按优先级顺序应用（从高到低）：

| 优先级 | 位置 | 范围 | 共享？ | 用途 |
|----------|----------|-------|---------|---------|
| 1 | 管理设置 | 组织 | 是（IT 部署） | 无法覆盖的安全策略 |
| 2 | 命令行参数 | 会话 | N/A | 临时单会话覆盖 |
| 3 | `.claude/settings.local.json` | 项目 | 否（git 忽略） | 个人项目特定 |
| 4 | `.claude/settings.json` | 项目 | 是（提交） | 团队共享设置 |
| 5 | `~/.claude/settings.json` | 用户 | N/A | 全局个人默认 |

**管理设置** 是组织强制执行的，无法被任何其他层级覆盖，包括命令行参数。交付方式：
- **服务器管理** 设置（远程交付）
- **MDM profiles** — macOS plist 位于 `com.anthropic.claudecode`
- **注册表策略** — Windows `HKLM\SOFTWARE\Policies\ClaudeCode`（管理员）和 `HKCU\SOFTWARE\Policies\ClaudeCode`（用户级，最低策略优先级）
- **文件** — `managed-settings.json`（macOS: `/Library/Application Support/ClaudeCode/`，Linux/WSL: `/etc/claude-code/`，Windows: `C:\Program Files\ClaudeCode\`)
- **Drop-in 目录** — `managed-settings.d/` 与 `managed-settings.json` 并列，用于独立策略片段（v2.1.83）。遵循 systemd 约定，`managed-settings.json` 先合并作为基础，然后 drop-in 目录中所有 `*.json` 文件按字母顺序排序并在上层合并。后续文件覆盖之前文件的标量值；数组拼接并去重；对象深度合并。以 `.` 开头的隐藏文件被忽略。使用数字前缀控制合并顺序（如 `10-telemetry.json`、`20-security.json`）

在管理层级内，优先级为：服务器管理 > MDM/OS 级策略 > 文件基础（`managed-settings.d/*.json` + `managed-settings.json`）> HKCU 注册表（仅 Windows）。只使用一个管理来源；来源不跨层级合并。在文件基础层级内，drop-in 文件和基础文件一起合并。

> **注意：** 自 v2.1.75 起，已弃用的 Windows 回退路径 `C:\ProgramData\ClaudeCode\managed-settings.json` 已移除。使用 `C:\Program Files\ClaudeCode\managed-settings.json` 替代。

**重要**：
- `deny` 规则具有最高安全优先级，无法被低优先级的 allow/ask 规则覆盖。
- 管理设置可能锁定或覆盖本地行为，即使本地文件指定了不同值。
- 数组设置（如 `permissions.allow`）跨范围**拼接并去重** — 所有层级的条目合并，不替换。

---

## 核心配置

### 通用设置

| 键 | 类型 | 默认值 | 描述 |
|-----|------|---------|-------------|
| `$schema` | string | - | JSON Schema URL 用于 IDE 验证和自动补全（如 `"https://json.schemastore.org/claude-code-settings.json"`） |
| `model` | string | `"default"` | 覆盖默认模型。接受别名（`sonnet`、`opus`、`haiku`）或完整模型 ID |
| `agent` | string | - | 设置主对话的默认代理。值是 `.claude/agents/` 中的代理名称。也可通过 `--agent` CLI 标志使用 |
| `language` | string | `"english"` | Claude 的首选响应语言 |
| `cleanupPeriodDays` | number | `30` | 超过此天数不活跃的会话在启动时删除。设为 `0` 删除所有现有转录并完全禁用会话持久化（不写 `.jsonl` 文件，`/resume` 无对话，钩子收到空 `transcript_path`） |
| `autoUpdatesChannel` | string | `"latest"` | 发布渠道：`"stable"` 或 `"latest"` |
| `alwaysThinkingEnabled` | boolean | `false` | 为所有会话默认启用扩展思考 |
| `skipWebFetchPreflight` | boolean | `false` | 获取 URL 前跳过 WebFetch 黑名单检查 *(在 JSON schema 中，不在官方设置页面)* |
| `availableModels` | array | - | 限制用户通过 `/model`、`--model`、Config 工具或 `ANTHROPIC_MODEL` 可选择的模型。不影响默认选项。示例：`["sonnet", "haiku"]` |
| `fastModePerSessionOptIn` | boolean | `false` | 要求用户每会话单独启用快速模式 |
| `defaultShell` | string | `"bash"` | 输入框 `!` 命令的默认 shell。接受 `"bash"`（默认）或 `"powershell"`。设置 `"powershell"` 在 Windows 上将交互式 `!` 命令路由到 PowerShell。需要 `CLAUDE_CODE_USE_POWERSHELL_TOOL=1`（v2.1.84） |
| `includeGitInstructions` | boolean | `true` | 在系统提示词中包含 git 相关指令 |
| `voiceEnabled` | boolean | - | 启用按讲语音听写。运行 `/voice` 时自动写入。需要 Claude.ai 账户 |
| `showClearContextOnPlanAccept` | boolean | `false` | 在计划接受界面显示"清除上下文"选项。设为 `true` 恢复选项（自 v2.1.81 默认隐藏） |
| `disableDeepLinkRegistration` | string | - | 设为 `"disable"` 阻止 Claude Code 启动时向操作系统注册 `claude-cli://` 协议处理器。深度链接让外部工具通过 `claude-cli://open?q=...` 打开带预填提示词的 Claude Code 会话。用于协议处理器注册受限或单独管理的环境 |
| `feedbackSurveyRate` | number | - | 符合条件时会话质量调查出现的概率（0–1）。企业管理员可控制调查显示频率。示例：`0.05` = 5% 符合条件的会话 |

**示例：**
```json
{
  "model": "opus",
  "agent": "code-reviewer",
  "language": "japanese",
  "cleanupPeriodDays": 60,
  "autoUpdatesChannel": "stable",
  "alwaysThinkingEnabled": true
}
```

### 计划与记忆目录

在自定义位置存储计划和自动记忆文件。

| 键 | 类型 | 默认值 | 描述 |
|-----|------|---------|-------------|
| `plansDirectory` | string | `~/.claude/plans` | `/plan` 输出存储的目录 |
| `autoMemoryDirectory` | string | - | 自动记忆存储的自定义目录。接受 `~/` 展开路径。不接受项目设置（`.claude/settings.json`）以防止将记忆写入敏感位置；接受策略、本地和用户设置 |

**示例：**
```json
{
  "plansDirectory": "./my-plans"
}
```

**用途：** 用于将计划工件与 Claude 内部文件分开组织，或将计划保存在团队共享位置。

### Worktree 设置

配置 `--worktree` 如何创建和管理 git worktree。用于在大型 monorepo 减少磁盘使用和启动时间。

| 键 | 类型 | 默认值 | 描述 |
|-----|------|---------|-------------|
| `worktree.symlinkDirectories` | array | `[]` | 从主仓库符号链接到每个 worktree 的目录，避免磁盘上重复大目录 |
| `worktree.sparsePaths` | array | `[]` | 通过 git sparse-checkout（cone 模式）在每个 worktree 检出的目录。只有列出的路径写入磁盘 |

**示例：**
```json
{
  "worktree": {
    "symlinkDirectories": ["node_modules", ".cache"],
    "sparsePaths": ["packages/my-app", "shared/utils"]
  }
}
```

### 归因设置

自定义 git 提交和 pull request 的归因消息。

| 键 | 类型 | 默认值 | 描述 |
|-----|------|---------|-------------|
| `attribution.commit` | string | Co-authored-by | Git 提交归因（支持 trailers） |
| `attribution.pr` | string | Generated message | Pull request 描述归因 |
| `includeCoAuthoredBy` | boolean | `true` | **已弃用** - 使用 `attribution` 替代 |

**示例：**
```json
{
  "attribution": {
    "commit": "Generated with AI\n\nCo-Authored-By: Claude <noreply@anthropic.com>",
    "pr": "Generated with Claude Code"
  }
}
```

**注意：** 设为空字符串（`""`）完全隐藏归因。

### 认证助手

动态认证令牌生成的脚本。

| 键 | 类型 | 描述 |
|-----|------|-------------|
| `apiKeyHelper` | string | 输出认证令牌的 shell 脚本路径（作为 `X-Api-Key` header 发送） |
| `forceLoginMethod` | string | 限制登录为 `"claudeai"` 或 `"console"` 账户 |
| `forceLoginOrgUUID` | string | 登录时自动选择组织的 UUID |

**示例：**
```json
{
  "apiKeyHelper": "/bin/generate_temp_api_key.sh",
  "forceLoginMethod": "console",
  "forceLoginOrgUUID": "xxxxxxxx-xxxx-xxxx-xxxx-xxxxxxxxxxxx"
}
```

### 公司公告

启动时向用户显示自定义公告（随机循环）。

| 键 | 类型 | 描述 |
|-----|------|-------------|
| `companyAnnouncements` | array | 启动时显示的字符串数组 |

**示例：**
```json
{
  "companyAnnouncements": [
    "Welcome to Acme Corp!",
    "Remember to run tests before committing!",
    "Check the wiki for coding standards"
  ]
}
```

---

## 权限

控制 Claude 可执行的工具和操作。

### 权限结构

```json
{
  "permissions": {
    "allow": [],
    "ask": [],
    "deny": [],
    "additionalDirectories": [],
    "defaultMode": "acceptEdits",
    "disableBypassPermissionsMode": "disable"
  }
}
```

### 权限键

| 键 | 类型 | 描述 |
|-----|------|-------------|
| `permissions.allow` | array | 无需提示允许工具使用的规则 |
| `permissions.ask` | array | 需要用户确认的规则 |
| `permissions.deny` | array | 阻止工具使用的规则（最高优先级） |
| `permissions.additionalDirectories` | array | Claude 可访问的额外目录 |
| `permissions.defaultMode` | string | 默认权限模式。在远程环境，只有 `acceptEdits` 和 `plan` 被遵守（v2.1.70+） |
| `permissions.disableBypassPermissionsMode` | string | 阻止绕过模式激活 |
| `allowManagedPermissionRulesOnly` | boolean | **（仅管理）** 只有管理权限规则生效；用户/项目 `allow`、`ask`、`deny` 规则被忽略 |
| `allow_remote_sessions` | boolean | **（仅管理）** 允许用户启动远程控制和网页会话。默认 `true`。设为 `false` 阻止远程会话访问 *(不在官方文档 — 官方权限页面称"远程控制和网页会话访问不由管理设置键控制。"团队和企业计划管理员通过 [Claude Code admin settings](https://claude.ai/admin-settings/claude-code) 启用/禁用)* |
| `autoMode` | object | 自定义 [auto mode](/en/permission-modes#eliminate-prompts-with-auto-mode) 分类器阻止和允许的内容。包含 `environment`（可信基础设施描述）、`allow`（阻止规则例外）和 `soft_deny`（阻止规则）— 都是散文字符串数组。**不从共享项目设置读取**（`.claude/settings.json`）防止仓库注入。在用户、本地和管理设置可用。设置 `allow` 或 `soft_deny` **替换**该部分的整个默认列表。运行 `claude auto-mode defaults` 查看内置规则再自定义 |
| `disableAutoMode` | string | 设为 `"disable"` 阻止 [auto mode](/en/permission-modes#eliminate-prompts-with-auto-mode) 激活。从 `Shift+Tab` 循环移除 `auto` 并在启动时拒绝 `--permission-mode auto`。可在任何设置层级设置；在管理设置中最有用，用户无法覆盖 |
| `useAutoModeDuringPlan` | boolean | 计划模式在 auto mode 可用时是否使用 auto mode 语义。默认：`true`。不从共享项目设置读取（`.claude/settings.json`）。在 `/config` 显示为"Use auto mode during plan" |

### 权限模式

| 模式 | 行为 |
|------|----------|
| `"default"` | 带提示的标准权限检查 |
| `"acceptEdits"` | 无需询问自动接受文件编辑 |
| `"askEdits"` | 每次操作前询问 *(不在官方文档 — 未验证)* |
| `"dontAsk"` | 除非通过 `/permissions` 或 `permissions.allow` 规则预批准，否则自动拒绝工具 |
| `"viewOnly"` | 只读模式，无修改 *(不在官方文档 — 未验证)* |
| `"bypassPermissions"` | 跳过所有权限检查（危险） |
| `"auto"` | 后台分类器替代手动提示。研究预览 — 需要团队计划 + Sonnet/Opus 4.6。分类器自动批准只读和文件编辑；其他全部通过安全检查。3 次连续或总共 20 次阻止后回退到提示。用 `autoMode` 设置配置 |
| `"plan"` | 只读探索模式 |

### 工具权限语法

| 工具 | 语法 | 示例 |
|------|--------|----------|
| `Bash` | `Bash(command pattern)` | `Bash(npm run *)`, `Bash(* install)`, `Bash(git * main)` |
| `Read` | `Read(path pattern)` | `Read(.env)`, `Read(./secrets/**)` |
| `Edit` | `Edit(path pattern)` | `Edit(src/**)`, `Edit(*.ts)` |
| `Write` | `Write(path pattern)` | `Write(*.md)`, `Write(./docs/**)` |
| `NotebookEdit` | `NotebookEdit(pattern)` | `NotebookEdit(*)` |
| `WebFetch` | `WebFetch(domain:pattern)` | `WebFetch(domain:example.com)` |
| `WebSearch` | `WebSearch` | 全局网页搜索 |
| `Task` | `Task(agent-name)` | `Task(Explore)`, `Task(my-agent)` |
| `Agent` | `Agent(name)` | `Agent(researcher)`, `Agent(*)` — 权限范围限于子代理生成 |
| `Skill` | `Skill(skill-name)` | `Skill(weather-fetcher)` |
| `MCP` | `mcp__server__tool` 或 `MCP(server:tool)` | `mcp__memory__*`, `MCP(github:*)` |

**评估顺序：** 规则按顺序评估：先 deny 规则，然后 ask，然后 allow。第一个匹配的规则胜出。

**Read/Edit 路径模式：** `Read`、`Edit` 和 `Write` 的权限规则支持 gitignore 风格模式，有四种前缀类型：

| 前缀 | 含义 | 示例 |
|--------|---------|-------------|
| `//` | 从文件系统根的绝对路径 | `Read(//Users/alice/file)` |
| `~/` | 相对主目录 | `Read(~/.zshrc)` |
| `/` | 相对项目根 | `Edit(/src/**)` |
| `./` 或无 | 相对路径（当前目录） | `Read(.env)`, `Read(*.ts)` |

**Bash 通配符注意：**
- `*` 可出现在**任何位置**：前缀（`Bash(* install)`）、后缀（`Bash(npm *)`）或中间（`Bash(git * main)`）
- **单词边界：** `Bash(ls *)`（`*` 前有空格）匹配 `ls -la` 但不匹配 `lsof`；`Bash(ls*)`（无空格）匹配两者
- `Bash(*)` 视为等效于 `Bash`（匹配所有 bash 命令）
- 权限规则支持输出重定向：`Bash(python:*)` 匹配 `python script.py > output.txt`
- 旧版 `:*` 后缀语法（如 `Bash(npm:*)`）等效于 ` *` 但已弃用

**示例：**
```json
{
  "permissions": {
    "allow": [
      "Edit(*)",
      "Write(*)",
      "Bash(npm run *)",
      "Bash(git *)",
      "WebFetch(domain:*)",
      "mcp__*"
    ],
    "ask": [
      "Bash(rm *)",
      "Bash(git push *)"
    ],
    "deny": [
      "Read(.env)",
      "Read(./secrets/**)",
      "Bash(curl *)"
    ],
    "additionalDirectories": ["../shared-libs/"]
  }
}
```

---

## 钩子

钩子配置（事件、属性、匹配器、退出码、环境变量和 HTTP 钩子）在专用仓库维护：

> **[claude-code-hooks](https://github.com/shanraisshan/claude-code-hooks)** — 完整钩子参考，包含声音通知系统、全部 19 个钩子事件、HTTP 钩子、匹配器模式、退出码和环境变量。

钩子相关设置键（`hooks`、`disableAllHooks`、`allowManagedHooksOnly`、`allowedHttpHookUrls`、`httpHookAllowedEnvVars`）在那里文档。

官方钩子参考见 [Claude Code Hooks Documentation](https://code.claude.com/docs/en/hooks)。

---

## MCP 服务器

配置模型上下文协议服务器以扩展能力。

### MCP 设置

| 键 | 类型 | 范围 | 描述 |
|-----|------|-------|-------------|
| `enableAllProjectMcpServers` | boolean | Any | 自动审批所有 `.mcp.json` 服务器 |
| `enabledMcpjsonServers` | array | Any | 白名单特定服务器名称 |
| `disabledMcpjsonServers` | array | Any | 黑名单特定服务器名称 |
| `allowedMcpServers` | array | 仅管理 | 带名称/命令/URL 匹配的白名单 |
| `deniedMcpServers` | array | 仅管理 | 带匹配的黑名单 |
| `allowManagedMcpServersOnly` | boolean | 仅管理 | 只允许管理白名单明确列出的 MCP 服务器 |
| `channelsEnabled` | boolean | 仅管理 | 允许团队和企业用户的 [channels](https://code.claude.com/docs/en/channels)。未设置或 `false` 时，无论 `--channels` 标志如何都阻止渠道消息交付 |
| `allowedChannelPlugins` | array | 仅管理 | 可推送消息的渠道插件白名单。设置时替换默认 Anthropic 白名单。未定义 = 回退默认，空数组 = 阻止所有渠道插件。需要 `channelsEnabled: true`。每条是带 `marketplace` 和 `plugin` 字段的对象（v2.1.84） |

### MCP 服务器匹配（管理设置）

```json
{
  "allowedMcpServers": [
    { "serverName": "github" },
    { "serverCommand": "npx @modelcontextprotocol/*" },
    { "serverUrl": "https://mcp.company.com/*" }
  ],
  "deniedMcpServers": [
    { "serverName": "dangerous-server" }
  ]
}
```

**示例：**
```json
{
  "enableAllProjectMcpServers": true,
  "enabledMcpjsonServers": ["memory", "github", "filesystem"],
  "disabledMcpjsonServers": ["experimental-server"]
}
```

---

## 沙箱

配置 bash 命令沙箱以提高安全性。

### 沙箱设置

| 键 | 类型 | 默认值 | 描述 |
|-----|------|---------|-------------|
| `sandbox.enabled` | boolean | `false` | 启用 bash 沙箱 |
| `sandbox.failIfUnavailable` | boolean | `false` | 沙箱启用但无法启动时退出报错，而不是无沙箱运行。用于要求严格沙箱的企业策略（v2.1.83） |
| `sandbox.autoAllowBashIfSandboxed` | boolean | `true` | 沙箱时自动批准 bash |
| `sandbox.excludedCommands` | array | `[]` | 在沙箱外运行的命令 |
| `sandbox.allowUnsandboxedCommands` | boolean | `true` | 允许 `dangerouslyDisableSandbox` |
| `sandbox.ignoreViolations` | object | `{}` | 命令模式到路径数组的映射 — 抑制违规警告 *(在 JSON schema 中，不在官方设置页面)* |
| `sandbox.enableWeakerNestedSandbox` | boolean | `false` | Docker 用较弱沙箱（降低安全性） |
| `sandbox.network.allowUnixSockets` | array | `[]` | 沙箱可访问的特定 Unix socket 路径 |
| `sandbox.network.allowAllUnixSockets` | boolean | `false` | 允许所有 Unix sockets（覆盖 allowUnixSockets） |
| `sandbox.network.allowLocalBinding` | boolean | `false` | 允许绑定到 localhost 端口（macOS） |
| `sandbox.network.allowedDomains` | array | `[]` | 沙箱网络域名白名单 |
| `sandbox.network.deniedDomains` | array | `[]` | 沙箱网络域名黑名单 *(不在官方文档 — 未验证)* |
| `sandbox.network.httpProxyPort` | number | - | HTTP 代理端口 1-65535（自定义代理） |
| `sandbox.network.socksProxyPort` | number | - | SOCKS5 代理端口 1-65535（自定义代理） |
| `sandbox.network.allowManagedDomainsOnly` | boolean | `false` | 只允许管理白名单中的域名（管理设置） |
| `sandbox.filesystem.allowWrite` | array | `[]` | 沙箱命令可写入的额外路径。数组跨所有设置范围拼接。前缀：`/`（绝对）、`~/`（主目录）、`./` 或无（项目设置中项目相对，用户设置中 `~/.claude` 相对）。旧版 `//` 绝对路径前缀仍有效。**注意：** 这与 [Read/Edit 权限规则](#工具权限语法)不同，后者用 `//` 表示绝对，`/` 表示项目相对 |
| `sandbox.filesystem.denyWrite` | array | `[]` | 沙箱命令不可写入的路径。数组跨所有设置范围拼接。路径前缀约定同 `allowWrite` |
| `sandbox.filesystem.denyRead` | array | `[]` | 沙箱命令不可读取的路径。数组跨所有设置范围拼接。路径前缀约定同 `allowWrite` |
| `sandbox.filesystem.allowRead` | array | `[]` | 在 `denyRead` 区域内重新允许读取的路径。优先于 `denyRead`。数组跨所有设置范围拼接。路径前缀约定同 `allowWrite` |
| `sandbox.filesystem.allowManagedReadPathsOnly` | boolean | `false` | **（仅管理）** 只有管理设置的 `allowRead` 路径生效。用户、项目和本地设置的 `allowRead` 条目被忽略 |
| `sandbox.enableWeakerNetworkIsolation` | boolean | `false` | （仅 macOS）允许访问系统 TLS 信任（`com.apple.trustd.agent`）；降低安全性 |

**示例：**
```json
{
  "sandbox": {
    "enabled": true,
    "autoAllowBashIfSandboxed": true,
    "excludedCommands": ["git", "docker", "gh"],
    "allowUnsandboxedCommands": false,
    "network": {
      "allowUnixSockets": ["/var/run/docker.sock"],
      "allowLocalBinding": true
    }
  }
}
```

---

## 插件

配置 Claude Code 插件和市场。

### 插件设置

| 键 | 类型 | 范围 | 描述 |
|-----|------|-------|-------------|
| `enabledPlugins` | object | Any | 启用/禁用特定插件 |
| `extraKnownMarketplaces` | object | Project | 添加自定义插件市场（通过 `.claude/settings.json` 团队共享） |
| `strictKnownMarketplaces` | array | 仅管理 | 允许的市场白名单 |
| `skippedMarketplaces` | array | Any | 用户拒绝安装的市场 *(在 JSON schema 中，不在官方设置页面)* |
| `skippedPlugins` | array | Any | 用户拒绝安装的插件 *(在 JSON schema 中，不在官方设置页面)* |
| `pluginConfigs` | object | Any | 每插件 MCP 服务器配置（按 `plugin@marketplace` 键） *(在 JSON schema 中，不在官方设置页面)* |
| `blockedMarketplaces` | array | 仅管理 | 阻止特定插件市场 |
| `pluginTrustMessage` | string | 仅管理 | 提示用户信任插件时显示的自定义消息 |

**市场来源类型：** `github`、`git`、`directory`、`hostPattern`、`settings`、`url`、`npm`、`file`。使用 `source: 'settings'` 内联声明小量插件，无需设置托管市场仓库。

**示例：**
```json
{
  "enabledPlugins": {
    "formatter@acme-tools": true,
    "deployer@acme-tools": true,
    "experimental@acme-tools": false
  },
  "extraKnownMarketplaces": {
    "acme-tools": {
      "source": {
        "source": "github",
        "repo": "acme-corp/claude-plugins"
      }
    },
    "inline-tools": {
      "source": {
        "source": "settings",
        "name": "inline-tools",
        "plugins": [
          {
            "name": "code-formatter",
            "source": { "source": "github", "repo": "acme-corp/code-formatter" }
          }
        ]
      }
    }
  }
}
```

---

## 模型配置

### 模型别名

| 别名 | 描述 |
|-------|-------------|
| `"default"` | 为你的账户类型推荐 |
| `"sonnet"` | 最新 Sonnet 模型（Claude Sonnet 4.6） |
| `"opus"` | 最新 Opus 模型（Claude Opus 4.6） |
| `"haiku"` | 快速 Haiku 模型 |
| `"sonnet[1m]"` | 带 1M token 上下文的 Sonnet |
| `"opus[1m]"` | 带 1M token 上下文的 Opus（自 v2.1.75 Max、Team 和 Enterprise 默认） |
| `"opusplan"` | 规划用 Opus，执行用 Sonnet |

**示例：**
```json
{
  "model": "opus"
}
```

### 模型覆盖

将 Anthropic 模型 ID 映射到 Bedrock、Vertex 或 Foundry 部署的提供商特定模型 ID。

| 键 | 类型 | 默认值 | 描述 |
|-----|------|---------|-------------|
| `effortLevel` | string | - | 跨会话持久化努力级别。接受 `"low"`、`"medium"` 或 `"high"`。运行 `/effort low`、`/effort medium` 或 `/effort high` 时自动写入。Opus 4.6 和 Sonnet 4.6 支持 |
| `modelOverrides` | object | - | 将模型选择器条目映射到提供商特定 ID（如 Bedrock inference profile ARN）。每个键是模型选择器条目名称，每个值是提供商模型 ID |

**示例：**
```json
{
  "modelOverrides": {
    "claude-opus-4-6": "arn:aws:bedrock:us-east-1:123456789:inference-profile/anthropic.claude-opus-4-6-v1:0",
    "claude-sonnet-4-6": "arn:aws:bedrock:us-east-1:123456789:inference-profile/anthropic.claude-sonnet-4-6-v1:0"
  }
}
```

### 努力级别

`/model` 命令暴露一个**努力级别**控制，调整模型每次响应应用的推理量。在 `/model` UI 中使用 ← → 箭头键循环努力级别。

| 努力级别 | 描述 |
|-------------|-------------|
| High | 完整推理深度，最适合复杂任务 |
| Medium（默认） | 平衡推理，适合日常任务 |
| Low | 最少推理，最快响应 |

**使用方法：**
1. 运行 `/effort low`、`/effort medium` 或 `/effort high` 直接设置（v2.1.76+）
2. 或运行 `/model` → 选择模型 → 使用 **← →** 箭头键调整
3. 设置通过 `settings.json` 中 `effortLevel` 键持久化

**注意：** 努力级别适用于 Max 和 Team 计划的 Opus 4.6 和 Sonnet 4.6。默认值在 v2.1.68 从 High 改为 Medium。自 v2.1.75 起，Opus 4.6 的 1M 上下文窗口在 Max、Team 和 Enterprise 计划默认可用。

### 模型环境变量

通过 `env` 键配置：

```json
{
  "env": {
    "ANTHROPIC_MODEL": "sonnet",
    "ANTHROPIC_DEFAULT_HAIKU_MODEL": "custom-haiku-model",
    "ANTHROPIC_DEFAULT_SONNET_MODEL": "custom-sonnet-model",
    "ANTHROPIC_DEFAULT_OPUS_MODEL": "custom-opus-model",
    "CLAUDE_CODE_SUBAGENT_MODEL": "haiku",
    "MAX_THINKING_TOKENS": "10000"
  }
}
```

---

## 显示与 UX

### 显示设置

| 键 | 类型 | 默认值 | 描述 |
|-----|------|---------|-------------|
| `statusLine` | object | - | 自定义状态栏配置 |
| `outputStyle` | string | `"default"` | 输出风格（如 `"Explanatory"`） |
| `spinnerTipsEnabled` | boolean | `true` | 等待时显示提示 |
| `spinnerVerbs` | object | - | 带 `mode`（"append" 或 "replace"）和 `verbs` 数组的自定义 spinner 动词 |
| `spinnerTipsOverride` | object | - | 带 `tips`（字符串数组）和可选 `excludeDefault`（boolean）的自定义 spinner 提示 |
| `respectGitignore` | boolean | `true` | 文件选择器尊重 .gitignore |
| `prefersReducedMotion` | boolean | `false` | 减少 UI 动画和动态效果 |
| `fileSuggestion` | object | - | 自定义文件建议命令（见下方文件建议配置） |

### 全局配置设置（`~/.claude.json`）

这些显示偏好存储在 `~/.claude.json`，**不是** `settings.json`。添加到 `settings.json` 会触发 schema 验证错误。

| 键 | 类型 | 默认值 | 描述 |
|-----|------|---------|-------------|
| `autoConnectIde` | boolean | `false` | Claude Code 从外部终端启动时自动连接到运行中的 IDE。在外部终端运行时在 `/config` 显示为 **Auto-connect to IDE (external terminal)** |
| `autoInstallIdeExtension` | boolean | `true` | 从 VS Code 终端运行时自动安装 Claude Code IDE 扩展。在 `/config` 显示为 **Auto-install IDE extension**。也可通过 `CLAUDE_CODE_IDE_SKIP_AUTO_INSTALL` env var 禁用 |
| `editorMode` | string | `"normal"` | 输入提示键绑定模式：`"normal"` 或 `"vim"`。运行 `/vim` 时自动写入。在 `/config` 显示为 **Key binding mode** |
| `showTurnDuration` | boolean | `true` | 响应后显示轮次时长消息（如 "Cooked for 1m 6s"）。直接编辑 `~/.claude.json` 更改 |
| `terminalProgressBarEnabled` | boolean | `true` | 在支持的终端显示终端进度条（ConEmu、Ghostty 1.2.0+ 和 iTerm2 3.6.6+）。在 `/config` 显示为 **Terminal progress bar** |
| `teammateMode` | string | `"auto"` | [代理团队](https://code.claude.com/docs/en/agent-teams) 成员显示方式：`"auto"`（在 tmux 或 iTerm2 选分屏，否则 in-process）、`"in-process"` 或 `"tmux"`。参见 [选择显示模式](https://code.claude.com/docs/en/agent-teams#choose-a-display-mode) |

### 状态栏配置

```json
{
  "statusLine": {
    "type": "command",
    "command": "~/.claude/statusline.sh",
    "padding": 0
  }
}
```

**状态栏输入字段：**

状态栏命令通过 stdin 接收 JSON 对象，包含这些重要字段：

| 字段 | 描述 |
|-------|-------------|
| `workspace.added_dirs` | 通过 `/add-dir` 添加的目录 |
| `context_window.used_percentage` | 上下文窗口使用百分比 |
| `context_window.remaining_percentage` | 上下文窗口剩余百分比 |
| `current_usage` | 当前上下文窗口 token 计数 |
| `exceeds_200k_tokens` | 上下文是否超过 200k tokens |
| `rate_limits.five_hour.used_percentage` | 五小时速率限制使用百分比（v2.1.80+） |
| `rate_limits.five_hour.resets_at` | 五小时速率限制重置时间戳 |
| `rate_limits.seven_day.used_percentage` | 七天速率限制使用百分比 |
| `rate_limits.seven_day.resets_at` | 七天速率限制重置时间戳 |

### 文件建议配置

文件建议脚本通过 stdin 接收 JSON 对象（如 `{"query": "src/comp"}`）并必须输出最多 15 个文件路径（每行一个）。

```json
{
  "fileSuggestion": {
    "type": "command",
    "command": "~/.claude/file-suggestion.sh"
  },
  "respectGitignore": true
}
```

**示例：**
```json
{
  "statusLine": {
    "type": "command",
    "command": "git branch --show-current 2>/dev/null || echo 'no-branch'"
  },
  "spinnerTipsEnabled": true,
  "spinnerVerbs": {
    "mode": "replace",
    "verbs": ["Cooking", "Brewing", "Crafting", "Conjuring"]
  },
  "spinnerTipsOverride": {
    "tips": ["Use /compact at ~50% context", "Start with plan mode for complex tasks"],
    "excludeDefault": true
  }
}
```

---

## AWS 与云凭证

### AWS 设置

| 键 | 类型 | 描述 |
|-----|------|-------------|
| `awsAuthRefresh` | string | 刷新 AWS 认证的脚本（修改 `.aws` 目录） |
| `awsCredentialExport` | string | 输出带 AWS 凭证 JSON 的脚本 |

**示例：**
```json
{
  "awsAuthRefresh": "aws sso login --profile myprofile",
  "awsCredentialExport": "/bin/generate_aws_grant.sh"
}
```

### OpenTelemetry

| 键 | 类型 | 描述 |
|-----|------|-------------|
| `otelHeadersHelper` | string | 生成动态 OpenTelemetry headers 的脚本 |

**示例：**
```json
{
  "otelHeadersHelper": "/bin/generate_otel_headers.sh"
}
```

---

## 环境变量（via `env`）

为所有 Claude Code 会话设置环境变量。

```json
{
  "env": {
    "ANTHROPIC_API_KEY": "...",
    "NODE_ENV": "development",
    "DEBUG": "true"
  }
}
```

### 常用环境变量

| 变量 | 描述 |
|----------|-------------|
| `ANTHROPIC_API_KEY` | 认证用的 API key |
| `ANTHROPIC_AUTH_TOKEN` | OAuth 令牌 |
| `ANTHROPIC_BASE_URL` | 自定义 API endpoint |
| `ANTHROPIC_CUSTOM_MODEL_OPTION` | 在 `/model` 选择器添加为自定义条目的模型 ID。用于让非标准或网关特定模型可选择，不替换内置别名 |
| `ANTHROPIC_CUSTOM_MODEL_OPTION_NAME` | `/model` 选择器中自定义模型条目的显示名称。未设置时默认为模型 ID |
| `ANTHROPIC_CUSTOM_MODEL_OPTION_DESCRIPTION` | `/model` 选择器中自定义模型条目的显示描述。未设置时默认为 `Custom model (<model-id>)` |
| `ANTHROPIC_MODEL` | 要使用的模型名称。接受别名（`sonnet`、`opus`、`haiku`）或完整模型 ID。覆盖 `model` 设置 |
| `ANTHROPIC_DEFAULT_HAIKU_MODEL` | 用自定义模型 ID 覆盖 Haiku 模型别名（如用于第三方部署） |
| `ANTHROPIC_DEFAULT_HAIKU_MODEL_NAME` | 在 Bedrock/Vertex/Foundry 上使用固定模型时自定义 `/model` 选择器中 Haiku 条目标签。默认为模型 ID |
| `ANTHROPIC_DEFAULT_HAIKU_MODEL_DESCRIPTION` | 自定义 `/model` 选择器中 Haiku 条目描述。默认为 `Custom model (<model-id>)` |
| `ANTHROPIC_DEFAULT_HAIKU_MODEL_SUPPORTED_CAPABILITIES` | 覆盖固定 Haiku 模型的能力检测。逗号分隔值（如 `effort,thinking`）。当固定模型支持自动检测无法确认的功能时需要 |
| `CLAUDECODE` | 在 Claude Code 生成的 shell 环境（Bash 工具、tmux 会话）中设为 `1`。钩子或状态栏命令中不设置。用于检测脚本是否在 Claude Code shell 内运行 |
| `CLAUDE_CODE_SKIP_FAST_MODE_NETWORK_ERRORS` | 设为 `1` 当组织状态检查因网络错误失败时允许快速模式。用于企业代理阻止状态 endpoint 时 |
| `CLAUDE_CODE_USE_BEDROCK` | 使用 AWS Bedrock（`1` 启用） |
| `CLAUDE_CODE_USE_VERTEX` | 使用 Google Vertex AI（`1` 启用） |
| `CLAUDE_CODE_USE_FOUNDRY` | 使用 Microsoft Foundry（`1` 启用） |
| `CLAUDE_CODE_USE_POWERSHELL_TOOL` | 设为 `1` 在 Windows 启用 PowerShell 工具（预览 opt-in）。启用后 Claude 可原生运行 PowerShell 命令，而不是通过 Git Bash。仅支持原生 Windows，不支持 WSL（v2.1.84） |
| `CLAUDE_CODE_ENABLE_TELEMETRY` | 启用/禁用遥测（`0` 或 `1`） |
| `DISABLE_ERROR_REPORTING` | 禁用错误报告（`1` 禁用） |
| `DISABLE_TELEMETRY` | 禁用遥测（`1` 禁用） |
| `MCP_TIMEOUT` | MCP 启动超时（ms） |
| `MAX_MCP_OUTPUT_TOKENS` | MCP 最大输出 tokens（默认：25000）。输出超过 10000 tokens 时显示警告 |
| `BASH_MAX_TIMEOUT_MS` | Bash 命令超时 |
| `BASH_MAX_OUTPUT_LENGTH` | Bash 最大输出长度 |
| `CLAUDE_AUTOCOMPACT_PCT_OVERRIDE` | 自动压缩阈值百分比（1-100）。默认约 95%。设更低（如 `50`）更早触发压缩。高于 95% 无效。用 `/context` 监控当前使用。示例：`CLAUDE_AUTOCOMPACT_PCT_OVERRIDE=50 claude` |
| `CLAUDE_BASH_MAINTAIN_PROJECT_WORKING_DIR` | bash 调用间保持 cwd（`1` 启用） |
| `CLAUDE_CODE_DISABLE_BACKGROUND_TASKS` | 禁用后台任务（`1` 禁用） |
| `ENABLE_TOOL_SEARCH` | MCP 工具搜索阈值（如 `auto:5`） |
| `DISABLE_PROMPT_CACHING` | 禁用所有提示缓存（`1` 禁用） |
| `DISABLE_PROMPT_CACHING_HAIKU` | 禁用 Haiku 提示缓存 |
| `DISABLE_PROMPT_CACHING_SONNET` | 禁用 Sonnet 提示缓存 |
| `DISABLE_PROMPT_CACHING_OPUS` | 禁用 Opus 提示缓存 |
| `CLAUDE_CODE_DISABLE_EXPERIMENTAL_BETAS` | 禁用实验性 beta 功能（`1` 禁用） |
| `CLAUDE_CODE_SHELL` | 覆盖自动 shell 检测 |
| `CLAUDE_CODE_FILE_READ_MAX_OUTPUT_TOKENS` | 覆盖默认文件读取 token 限制 |
| `CLAUDE_CODE_ENABLE_TASKS` | 设为 `true` 在非交互模式（`-p` 标志）启用任务跟踪。交互模式默认启用任务 |
| `CLAUDE_CODE_EXIT_AFTER_STOP_DELAY` | SDK 空闲后自动退出延迟（ms） |
| `CLAUDE_CODE_DISABLE_ADAPTIVE_THINKING` | 禁用自适应思考（`1` 禁用） |
| `CLAUDE_CODE_DISABLE_1M_CONTEXT` | 禁用 1M token 上下文窗口（`1` 禁用） |
| `CLAUDE_CODE_ACCOUNT_UUID` | 覆盖认证账户 UUID |
| `CLAUDE_CODE_DISABLE_GIT_INSTRUCTIONS` | 禁用 git 相关系统提示指令 |
| `CLAUDE_CODE_NEW_INIT` | 设为 `true` 让 `/init` 运行交互式设置流程。先询问生成哪些文件（CLAUDE.md、skills、hooks）再探索代码库。无此设置，`/init` 自动生成 CLAUDE.md |
| `CLAUDE_CODE_PLUGIN_SEED_DIR` | 一个或多个只读插件种子目录路径，Unix 用 `:` 分隔，Windows 用 `;`。将预填充插件打包到容器镜像。Claude Code 启动时从这些目录注册市场并使用预缓存插件无需重新克隆 |
| `ENABLE_CLAUDEAI_MCP_SERVERS` | 启用 Claude.ai MCP 服务器 |
| `CLAUDE_CODE_EFFORT_LEVEL` | 设置努力级别：`low`、`medium`、`high`、`max`（仅 Opus 4.6）或 `auto`（用模型默认）。优先于 `/effort` 和 `effortLevel` 设置 |
| `CLAUDE_CODE_MAX_TURNS` | 停止前最大代理轮次 *(不在官方文档 — 未验证)* |
| `CLAUDE_CODE_DISABLE_NONESSENTIAL_TRAFFIC` | 等效于设置 `DISABLE_AUTOUPDATER`、`DISABLE_FEEDBACK_COMMAND`、`DISABLE_ERROR_REPORTING` 和 `DISABLE_TELEMETRY` |
| `CLAUDE_CODE_SKIP_SETTINGS_SETUP` | 跳过首次运行设置流程 *(不在官方文档 — 未验证)* |
| `CLAUDE_CODE_PROMPT_CACHING_ENABLED` | 覆盖提示缓存行为 *(不在官方文档 — 未验证)* |
| `CLAUDE_CODE_DISABLE_TOOLS` | 逗号分隔的禁用工具列表 *(不在官方文档 — 未验证)* |
| `CLAUDE_CODE_DISABLE_MCP` | 禁用所有 MCP 服务器（`1` 禁用） *(不在官方文档 — 未验证)* |
| `CLAUDE_CODE_MAX_OUTPUT_TOKENS` | 每响应最大输出 tokens。默认：32,000（v2.1.77 Opus 4.6 为 64,000）。上限：64,000（v2.1.77 Opus 4.6 和 Sonnet 4.6 为 128,000） |
| `CLAUDE_CODE_DISABLE_FAST_MODE` | 完全禁用快速模式（`1` 禁用） |
| `CLAUDE_CODE_DISABLE_NONSTREAMING_FALLBACK` | 设为 `1` 禁用流式请求中途失败时的非流式回退。流式错误传播到重试层。用于代理或网关导致回退产生重复工具执行时（v2.1.83） |
| `CLAUDE_CODE_DISABLE_AUTO_MEMORY` | 禁用自动记忆（`1` 禁用） |
| `CLAUDE_CODE_USER_EMAIL` | 同步提供用户邮箱用于认证 |
| `CLAUDE_CODE_ORGANIZATION_UUID` | 同步提供组织 UUID 用于认证 |
| `CLAUDE_CONFIG_DIR` | 自定义配置目录（覆盖默认 `~/.claude`） |
| `ANTHROPIC_CUSTOM_HEADERS` | API 请求自定义 headers（`Name: Value` 格式，多 header 换行分隔） |
| `ANTHROPIC_FOUNDRY_API_KEY` | Microsoft Foundry 认证 API key |
| `ANTHROPIC_FOUNDRY_BASE_URL` | Foundry 资源基础 URL |
| `ANTHROPIC_FOUNDRY_RESOURCE` | Foundry 资源名称 |
| `AWS_BEARER_TOKEN_BEDROCK` | Bedrock 认证 API key |
| `ANTHROPIC_SMALL_FAST_MODEL` | **已弃用** — 用 `ANTHROPIC_DEFAULT_HAIKU_MODEL` 替代 |
| `ANTHROPIC_SMALL_FAST_MODEL_AWS_REGION` | 已弃用 Haiku 级模型覆盖的 AWS 区域 |
| `CLAUDE_CODE_SHELL_PREFIX` | prepend 到 bash 命令的命令前缀 |
| `BASH_DEFAULT_TIMEOUT_MS` | 默认 bash 命令超时（ms） |
| `CLAUDE_CODE_SKIP_BEDROCK_AUTH` | 跳过 Bedrock AWS 认证（`1` 跳过） |
| `CLAUDE_CODE_SKIP_FOUNDRY_AUTH` | 跳过 Foundry Azure 认证（`1` 跳过） |
| `CLAUDE_CODE_SKIP_VERTEX_AUTH` | 跳过 Vertex Google 认证（`1` 跳过） |
| `CLAUDE_CODE_PROXY_RESOLVES_HOSTS` | 允许代理执行 DNS 解析 |
| `CLAUDE_CODE_API_KEY_HELPER_TTL_MS` | `apiKeyHelper` 凭证刷新间隔（ms） |
| `CLAUDE_CODE_CLIENT_CERT` | mTLS 客户端证书路径 |
| `CLAUDE_CODE_CLIENT_KEY` | mTLS 客户端私钥路径 |
| `CLAUDE_CODE_CLIENT_KEY_PASSPHRASE` | 加密 mTLS 密钥密码 |
| `CLAUDE_CODE_PLUGIN_GIT_TIMEOUT_MS` | 插件市场 git clone 超时（ms）（默认：120000） |
| `CLAUDE_CODE_HIDE_ACCOUNT_INFO` | UI 隐藏邮箱/组织信息 *(不在官方文档 — 未验证)* |
| `CLAUDE_CODE_DISABLE_CRON` | 禁用计划/cron 任务（`1` 禁用） |
| `DISABLE_INSTALLATION_CHECKS` | 禁用安装警告 |
| `DISABLE_FEEDBACK_COMMAND` | 禁用 `/feedback` 命令。旧名称 `DISABLE_BUG_COMMAND` 也接受 |
| `DISABLE_NON_ESSENTIAL_MODEL_CALLS` | 禁用风味文本和非必要模型调用 *(不在官方文档 — 未验证)* |
| `DISABLE_COST_WARNINGS` | 禁用成本警告消息 |
| `CLAUDE_CODE_SUBAGENT_MODEL` | 覆盖子代理模型（如 `haiku`、`sonnet`） |
| `CLAUDE_CODE_SUBPROCESS_ENV_SCRUB` | 设为 `1` 从子进程环境（Bash 工具、钩子、MCP stdio 服务器）清除 Anthropic 和云提供商凭证。用于深度防御，子进程不应继承 API key（v2.1.83） |
| `CLAUDE_CODE_SESSIONEND_HOOKS_TIMEOUT_MS` | SessionEnd 钩子超时（ms）（替代硬 1.5s 限制） |
| `CLAUDE_CODE_DISABLE_FEEDBACK_SURVEY` | 禁用反馈调查提示（`1` 禁用） |
| `CLAUDE_CODE_DISABLE_TERMINAL_TITLE` | 禁用终端标题更新（`1` 禁用） |
| `CLAUDE_CODE_IDE_SKIP_AUTO_INSTALL` | 跳过自动 IDE 扩展安装（`1` 跳过） |
| `CLAUDE_CODE_OTEL_HEADERS_HELPER_DEBOUNCE_MS` | OTel headers helper 脚本去抖间隔（ms） |
| `CLAUDE_STREAM_IDLE_TIMEOUT_MS` | 流式空闲看门狗关闭停滞连接前的超时（ms）。默认：`90000`（90 秒）。如果长时间运行工具或慢网络导致过早超时错误则增加 |
| `OTEL_LOG_TOOL_DETAILS` | 设为 `1` 在 OpenTelemetry 事件中包含 `tool_parameters`。默认省略以保护隐私 *(在 v2.1.85 changelog，未在官方 env-vars 页面)* |
| `CLAUDE_CODE_MCP_SERVER_NAME` | MCP 服务器名称，作为环境变量传递给 `headersHelper` 脚本以便生成服务器特定认证 headers *(在 v2.1.85 changelog，未在官方 env-vars 页面)* |
| `CLAUDE_CODE_MCP_SERVER_URL` | MCP 服务器 URL，与 `CLAUDE_CODE_MCP_SERVER_NAME` 一起传递给 `headersHelper` 脚本 *(在 v2.1.85 changelog，未在官方 env-vars 页面)* |
| `ANTHROPIC_DEFAULT_OPUS_MODEL` | 覆盖 Opus 模型别名（如 `claude-opus-4-6[1m]`） |
| `ANTHROPIC_DEFAULT_OPUS_MODEL_NAME` | 在 Bedrock/Vertex/Foundry 使用固定模型时自定义 `/model` 选择器中 Opus 条目标签。默认为模型 ID |
| `ANTHROPIC_DEFAULT_OPUS_MODEL_DESCRIPTION` | 自定义 `/model` 选择器中 Opus 条目描述。默认为 `Custom model (<model-id>)` |
| `ANTHROPIC_DEFAULT_OPUS_MODEL_SUPPORTED_CAPABILITIES` | 覆盖固定 Opus 模型的能力检测。逗号分隔值（如 `effort,thinking`）。当固定模型支持自动检测无法确认的功能时需要 |
| `ANTHROPIC_DEFAULT_SONNET_MODEL` | 覆盖 Sonnet 模型别名（如 `claude-sonnet-4-6`） |
| `ANTHROPIC_DEFAULT_SONNET_MODEL_NAME` | 在 Bedrock/Vertex/Foundry 使用固定模型时自定义 `/model` 选择器中 Sonnet 条目标签。默认为模型 ID |
| `ANTHROPIC_DEFAULT_SONNET_MODEL_DESCRIPTION` | 自定义 `/model` 选择器中 Sonnet 条目描述。默认为 `Custom model (<model-id>)` |
| `ANTHROPIC_DEFAULT_SONNET_MODEL_SUPPORTED_CAPABILITIES` | 覆盖固定 Sonnet 模型的能力检测。逗号分隔值（如 `effort,thinking`）。当固定模型支持自动检测无法确认的功能时需要 |
| `MAX_THINKING_TOKENS` | 每响应最大扩展思考 tokens |
| `CLAUDE_CODE_AUTO_COMPACT_WINDOW` | 设置自动压缩计算用的上下文容量（tokens）。默认为模型上下文窗口（200K 标准，1M 扩展上下文模型）。在 1M 模型上用更低值（如 `500000`）将其视为 500K 用于压缩。上限为实际上下文窗口。`CLAUDE_AUTOCOMPACT_PCT_OVERRIDE` 应用为此值的百分比。此设置将压缩阈值与状态栏 `used_percentage` 解耦 |
| `CLAUDE_CODE_ENABLE_PROMPT_SUGGESTION` | 启用提示建议 |
| `CLAUDE_CODE_PLAN_MODE_REQUIRED` | 要求会话使用计划模式 |
| `CLAUDE_CODE_TEAM_NAME` | 代理团队名称 |
| `CLAUDE_CODE_TASK_LIST_ID` | 任务集成的任务列表 ID |
| `CLAUDE_ENV_FILE` | 自定义环境文件路径 |
| `FORCE_AUTOUPDATE_PLUGINS` | 强制插件自动更新（`1` 启用） |
| `HTTP_PROXY` | 网络请求 HTTP 代理 URL |
| `HTTPS_PROXY` | 网络请求 HTTPS 代理 URL |
| `NO_PROXY` | 绕过代理的主机逗号分隔列表 |
| `MCP_TOOL_TIMEOUT` | MCP 工具执行超时（ms） |
| `MCP_CLIENT_SECRET` | MCP OAuth 客户端密钥 |
| `MCP_OAUTH_CALLBACK_PORT` | MCP OAuth 回调端口 |
| `IS_DEMO` | 启用演示模式 |
| `SLASH_COMMAND_TOOL_CHAR_BUDGET` | 斜杠命令工具输出的字符预算 |
| `VERTEX_REGION_CLAUDE_3_5_HAIKU` | Claude 3.5 Haiku Vertex AI 区域覆盖 |
| `VERTEX_REGION_CLAUDE_3_7_SONNET` | Claude 3.7 Sonnet Vertex AI 区域覆盖 |
| `VERTEX_REGION_CLAUDE_4_0_OPUS` | Claude 4.0 Opus Vertex AI 区域覆盖 |
| `VERTEX_REGION_CLAUDE_4_0_SONNET` | Claude 4.0 Sonnet Vertex AI 区域覆盖 |
| `VERTEX_REGION_CLAUDE_4_1_OPUS` | Claude 4.1 Opus Vertex AI 区域覆盖 |

---

## 实用命令

| 命令 | 描述 |
|---------|-------------|
| `/model` | 切换模型并调整 Opus 4.6 努力级别 |
| `/effort` | 直接设置努力级别：`low`、`medium`、`high`（v2.1.76+） |
| `/config` | 交互式配置 UI |
| `/memory` | 查看/编辑所有记忆文件 |
| `/agents` | 管理子代理 |
| `/mcp` | 管理 MCP 服务器 |
| `/hooks` | 查看配置的钩子 |
| `/plugin` | 管理插件 |
| `/keybindings` | 配置自定义键盘快捷键 |
| `/skills` | 查看和管理技能 |
| `/permissions` | 查看和管理权限规则 |
| `--doctor` | 诊断配置问题 |
| `--debug` | 调试模式含钩子执行详情 |

---

## 快速参考：完整示例

```json
{
  "$schema": "https://json.schemastore.org/claude-code-settings.json",
  "model": "sonnet",
  "agent": "code-reviewer",
  "language": "english",
  "cleanupPeriodDays": 30,
  "autoUpdatesChannel": "stable",
  "alwaysThinkingEnabled": true,
  "includeGitInstructions": true,
  "defaultShell": "bash",
  "plansDirectory": "./plans",
  "effortLevel": "medium",

  "worktree": {
    "symlinkDirectories": ["node_modules"],
    "sparsePaths": ["packages/my-app", "shared/utils"]
  },

  "modelOverrides": {
    "claude-opus-4-6": "arn:aws:bedrock:us-east-1:123456789:inference-profile/anthropic.claude-opus-4-6-v1:0"
  },

  "autoMode": {
    "environment": [
      "Source control: github.example.com/acme-corp and all repos under it",
      "Trusted internal domains: *.internal.example.com"
    ]
  },

  "permissions": {
    "allow": [
      "Edit(*)",
      "Write(*)",
      "Bash(npm run *)",
      "Bash(git *)",
      "WebFetch(domain:*)",
      "mcp__*",
      "Agent(*)"
    ],
    "deny": [
      "Read(.env)",
      "Read(./secrets/**)"
    ],
    "additionalDirectories": ["../shared/"],
    "defaultMode": "acceptEdits"
  },

  "enableAllProjectMcpServers": true,

  "sandbox": {
    "enabled": true,
    "excludedCommands": ["git", "docker"],
    "filesystem": {
      "denyRead": ["./secrets/"],
      "denyWrite": ["./.env"]
    }
  },

  "attribution": {
    "commit": "Generated with Claude Code",
    "pr": ""
  },

  "statusLine": {
    "type": "command",
    "command": "git branch --show-current"
  },

  "spinnerTipsEnabled": true,
  "spinnerTipsOverride": {
    "tips": ["Custom tip 1", "Custom tip 2"],
    "excludeDefault": false
  },
  "prefersReducedMotion": false,

  "env": {
    "NODE_ENV": "development",
    "CLAUDE_CODE_EFFORT_LEVEL": "medium"
  }
}
```

---

## 来源

- [Claude Code Settings Documentation](https://code.claude.com/docs/en/settings)
- [Claude Code Settings JSON Schema](https://json.schemastore.org/claude-code-settings.json)
- [Claude Code Changelog](https://github.com/anthropics/claude-code/blob/main/CHANGELOG.md)
- [Claude Code GitHub Settings Examples](https://github.com/feiskyer/claude-code-settings)
- [Eesel AI - Developer's Guide to settings.json](https://www.eesel.ai/blog/settings-json-claude-code)
- [Shipyard - Claude Code CLI Cheatsheet](https://shipyard.build/blog/claude-code-cheat-sheet/)
- [Claude Code Environment Variables Reference](https://code.claude.com/docs/en/env-vars)
- [Claude Code Permissions Reference](https://code.claude.com/docs/en/permissions)