# Claude Code: 代理记忆 Frontmatter

子代理的持久记忆 — 使代理能够跨会话学习、记住和构建知识。

<table width="100%">
<tr>
<td><a href="../">← 返回 Claude Code 最佳实践</a></td>
<td align="right"><img src="../!/claude-jumping.svg" alt="Claude" width="60" /></td>
</tr>
</table>

---

## 概述

在 **Claude Code v2.1.33**（2026 年 2 月）引入，`memory` frontmatter 字段为每个子代理提供自己的持久 markdown 知识存储。在此之前，每次代理调用都从零开始。

```yaml
---
name: code-reviewer
description: 审查代码质量和最佳实践
tools: Read, Write, Edit, Bash
model: sonnet
memory: user
---

你是一个代码审查器。在审查代码时，用你发现的模式、约定和反复出现的问题更新你的代理记忆。
```

---

## 记忆范围

| 范围 | 存储位置 | 版本控制 | 共享 | 最佳用于 |
|-------|-----------------|-------------------|--------|----------|
| `user` | `~/.claude/agent-memory/<agent-name>/` | 否 | 否 | 跨项目知识（推荐默认值） |
| `project` | `.claude/agent-memory/<agent-name>/` | 是 | 是 | 团队应共享的项目特定知识 |
| `local` | `.claude/agent-memory-local/<agent-name>/` | 否（git 忽略） | 否 | 个人项目特定知识 |

这些范围镜像了设置层次结构（`~/.claude/settings.json` → `.claude/settings.json` → `.claude/settings.local.json`）。

---

## 工作原理

1. **启动时**：`MEMORY.md` 的前 200 行被注入到代理的系统提示中
2. **工具访问**：`Read`、`Write`、`Edit` 自动启用以便代理管理其记忆
3. **执行期间**：代理自由读写其记忆目录
4. **整理**：如果 `MEMORY.md` 超过 200 行，代理将详情移入主题特定文件

```
~/.claude/agent-memory/code-reviewer/     # user 范围示例
├── MEMORY.md                              # 主文件（加载前 200 行）
├── react-patterns.md                      # 主题特定文件
└── security-checklist.md                  # 主题特定文件
```

---

## 代理记忆 vs 其他记忆系统

| 系统 | 谁写入 | 谁读取 | 范围 |
|--------|-----------|-----------|-------|
| **CLAUDE.md** | 你（手动） | 主 Claude + 所有代理 | 项目 |
| **自动记忆** | 主 Claude（自动） | 仅主 Claude | 每项目每用户 |
| **`/memory` 命令** | 你（通过编辑器） | 仅主 Claude | 每项目每用户 |
| **代理记忆** | 代理本身 | 仅该特定代理 | 可配置（user/project/local） |

这些系统是**互补的** — 代理同时读取 CLAUDE.md（项目上下文）和自己的记忆（代理特定知识）。

---

## 实际示例

```yaml
---
name: api-developer
description: 遵循团队约定实现 API 端点
tools: Read, Write, Edit, Bash
model: sonnet
memory: project
skills:
  - api-conventions
  - error-handling-patterns
---

实现 API 端点。遵循你预加载技能中的约定。
工作时，将架构决策和模式保存到你的记忆中。
```

这结合了**技能**（启动时的静态知识）和**记忆**（随时间构建的动态知识）。

---

## 技巧

- **提示记忆使用** — 包含显式指令：`"开始前，查看你的记忆。完成后，用你学到的内容更新你的记忆。"`
- 调用代理时**请求记忆检查**：`"审查这个 PR，并检查你的记忆中你以前见过的模式。"`
- **选择正确的范围** — `user` 用于跨项目，`project` 用于团队共享，`local` 用于个人

---

## 来源

- [创建自定义子代理 — Claude Code 文档](https://code.claude.com/docs/en/sub-agents)
- [管理 Claude 的记忆 — Claude Code 文档](https://code.claude.com/docs/en/memory)
- [Claude Code v2.1.33 发布说明](https://github.com/anthropics/claude-code/blob/main/CHANGELOG.md)