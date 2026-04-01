# 代理 vs 命令 vs 技能 — 何时使用什么

比较 Claude Code 中的三种扩展机制：子代理、命令和技能。

<table width="100%">
<tr>
<td><a href="../">← 返回 Claude Code 最佳实践</a></td>
<td align="right"><img src="../!/claude-jumping.svg" alt="Claude" width="60" /></td>
</tr>
</table>

![斜杠菜单显示 time-skill、time-command 和 time-agent](assets/agent-command-skill-1.jpg)

---

## 一览

| | 代理 | 命令 | 技能 |
|---|---|---|---|
| **位置** | `.claude/agents/<name>.md` | `.claude/commands/<name>.md` | `.claude/skills/<name>/SKILL.md` |
| **上下文** | 独立子代理进程 | 内联（主对话） | 内联（主对话） |
| **用户可调用** | 无 `/` 菜单 — 由 Claude 或通过 Agent 工具调用 | 是 — `/command-name` | 是 — `/skill-name`（除非 `user-invocable: false`） |
| **Claude 自动调用** | 是 — 通过 `description` 字段 | 否 | 是 — 通过 `description` 字段（除非 `disable-model-invocation: true`） |
| **接受参数** | 通过 `prompt` 参数 | `$ARGUMENTS`、`$0`、`$1` | `$ARGUMENTS`、`$0`、`$1` |
| **动态上下文注入** | 否 | 是 — `` !`command` `` | 是 — `` !`command` `` |
| **独立上下文窗口** | 是 — 隔离 | 否 — 共享主窗口 | 否 — 共享主窗口（除非 `context: fork`） |
| **模型覆盖** | `model:` frontmatter | `model:` frontmatter | `model:` frontmatter |
| **工具限制** | `tools:` / `disallowedTools:` | `allowed-tools:` | `allowed-tools:` |
| **钩子** | `hooks:` frontmatter | — | `hooks:` frontmatter |
| **记忆** | `memory:` frontmatter（user/project/local） | — | — |
| **可预加载技能** | 是 — `skills:` frontmatter | — | — |
| **MCP 服务器** | `mcpServers:` frontmatter | — | — |

---

## 何时使用每种

### 使用代理当：

- 任务是**自主和多步骤** — 代理需要探索、决定和行动，无需持续指导
- 你需要**上下文隔离** — 工作不应污染主对话窗口
- 代理需要跨会话的**持久记忆**（例如，学习模式的代码审查器）
- 你想通过技能**预加载领域知识**而不混乱主上下文
- 任务受益于**后台运行**或在 **git worktree** 中运行
- 你需要**工具限制**或**不同的权限模式**（如 `acceptEdits`、`plan`）

**示例**：`weather-agent` — 使用其预加载的 `weather-fetcher` 技能自主获取天气数据，在独立上下文中运行，工具受限。

### 使用命令当：

- 你需要一个**用户发起的入口点** — 用户显式触发的工作流
- 工作流涉及**编排**其他代理或技能
- 你想**保持上下文精简** — 命令内容在用户触发前不会注入到会话上下文

**示例**：`weather-orchestrator` — 用户触发它，它询问 C/F 偏好，调用代理，然后调用 SVG 技能。

### 使用技能当：

- 你想让 **Claude 根据用户意图自动调用** — 技能描述被注入到会话上下文进行语义匹配
- 任务是**可重用的过程**，可从多处调用（命令、代理或 Claude 本身）
- 你需要**代理预加载** — 在启动时将领域知识植入特定代理

**示例**：`weather-svg-creator` — 当用户要求天气卡片时 Claude 自动调用它；也可从命令调用。

---

## 命令 → 代理 → 技能架构

本仓库演示了分层编排模式：

```
用户触发 /command
    ↓
命令编排工作流
    ↓
命令调用代理（独立上下文，自主）
    ↓
代理使用预加载技能（领域知识）
    ↓
命令调用技能（内联，用于输出生成）
```

**具体示例** — 天气系统：

```
/weather-orchestrator（命令 — 入口点，询问 C/F）
    ↓
weather-agent（代理 — 自主获取温度）
    ├── weather-fetcher（代理技能 — 预加载的 API 指令）
    ↓
weather-svg-creator（技能 — 内联创建 SVG）
```

---

## Frontmatter 比较

### 代理 Frontmatter

```yaml
---
name: my-agent
description: 当...时主动使用此代理
tools: Read, Write, Edit, Bash
model: sonnet
maxTurns: 10
permissionMode: acceptEdits
memory: user
skills:
  - my-skill
---
```

### 命令 Frontmatter

```yaml
---
description: 做一些有用的事情
argument-hint: [issue-number]
allowed-tools: Read, Edit, Bash(gh *)
model: sonnet
---
```

### 技能 Frontmatter

```yaml
---
name: my-skill
description: 当用户请求...时做一些事情
argument-hint: [file-path]
disable-model-invocation: false
user-invocable: true
allowed-tools: Read, Grep, Glob
model: sonnet
context: fork
agent: general-purpose
---
```

---

## 关键区别

### 自动调用

| 机制 | Claude 可以自动调用？ | 如何阻止 |
|-----------|------------------------|----------------|
| 代理 | 是 — 通过 `description`（使用 "PROACTIVELY" 鼓励） | 删除或软化描述 |
| 命令 | 否 — 始终通过 `/` 用户发起 | N/A |
| 技能 | 是 — 通过 `description` | 设置 `disable-model-invocation: true` |

### 在 `/` 菜单中的可见性

| 机制 | 出现在 `/` 菜单？ | 如何隐藏 |
|-----------|---------------------|-------------|
| 代理 | 否 | N/A |
| 命令 | 是 — 始终 | 无法隐藏 |
| 技能 | 是 — 默认 | 设置 `user-invocable: false` |

### 上下文隔离

| 机制 | 在独立上下文中运行？ | 如何配置 |
|-----------|---------------------|-----------------|
| 代理 | 始终 | 内置行为 |
| 命令 | 从不 | N/A |
| 技能 | 可选 | 设置 `context: fork` |

---

## 实战示例："现在几点？"

本仓库为相同任务定义了所有三种机制 — 显示 PKT 当前时间。以下是用户输入 **"现在几点？"** 而未显式调用任何 `/` 命令时会发生什么：

| 机制 | 会触发吗？ | 为什么 / 为什么不 |
|-----------|--------------|---------------|
| `time-command` | 否 | 命令**从不被自动调用**。用户需要显式输入 `/time-command` 才能运行。命令没有自动发现路径 — 它们严格由用户发起。 |
| `time-agent` | **是**（可能） | 代理的 `description` 说 *"使用此代理显示巴基斯坦标准时间当前时间"*。Claude 将此与用户意图匹配，可能通过 Agent 工具生成它。但是，代理在**独立上下文窗口**中运行，使其对于此简单任务来说比必要的更重。 |
| `time-skill` | **是**（最可能） | 技能的 `description` 说 *"显示巴基斯坦标准时间（PKT，UTC+5）当前时间。当用户询问当前时间、巴基斯坦时间或 PKT 时使用。"* Claude 匹配此并通过 Skill 工具调用它。由于它**内联**运行，无上下文开销，是最高效的匹配。 |

### 解决顺序

当多种机制匹配相同意图时，Claude 偏好满足请求的**最轻量选项**：

```
1. 技能（内联，无上下文开销）     ← 首选
2. 代理（独立上下文，自主）       ← 如果技能不可用或任务复杂时使用
3. 命令（从不 — 需要显式 /）      ← 仅当用户输入 /time-command
```

### 如果技能设置了 `disable-model-invocation: true` 会怎样？

那么 Claude **无法**自动调用该技能。代理成为唯一可自动调用的选项，所以 Claude 会改为生成 `time-agent` — 代价是为单行 bash 命令使用独立上下文窗口。

### 如果技能和代理都禁用了自动调用会怎样？

那么**没有任何东西会自动触发**。Claude 会回退到自己的通用知识，可能直接运行 `TZ='Asia/Karachi' date` — 不涉及扩展机制。用户需要显式输入 `/time-command` 或 `/time-skill` 才能使用其中一个。

![Claude 在用户询问"现在几点？"时自动调用 time-skill](assets/agent-command-skill-2.png)

---

## 来源

- [Claude Code 技能 — 文档](https://code.claude.com/docs/en/skills)
- [Claude Code 子代理 — 文档](https://code.claude.com/docs/en/sub-agents)
- [Claude Code 斜杠命令 — 文档](https://code.claude.com/docs/en/slash-commands)
- [技能最佳实践](../best-practice/claude-skills.md)
- [命令最佳实践](../best-practice/claude-commands.md)
- [子代理最佳实践](../best-practice/claude-subagents.md)