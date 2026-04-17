# CLAUDE.md 优化方法

## 核心原则

来自 [presentation/learning-journey/index.zh.html](presentation/learning-journey/index.zh.html) 第18页的指导：

> **目标控制在 200 行以内。超过 200 行的文件会消耗更多上下文，可能降低遵从度。将详细说明放在 `.claude/rules/` 中。**

## 优化步骤

### 1. 分离职责

CLAUDE.md 应回答：**这是什么项目、我关心什么、我要避免什么**

不应包含：字段参考、架构图、技术规格

### 2. 提取技术规格到 Rules

将以下内容从 CLAUDE.md 移到 `.claude/rules/`：

- SKILL.md YAML 字段参考 → `.claude/rules/skill-fields.md`
- Agent YAML 字段参考 → `.claude/rules/agent-fields.md`
- 配置层级、Hook 系统 → `.claude/rules/config-hierarchy.md`

Rules 支持 `# Glob: pattern` 按需加载，只在与匹配文件一起工作时才加载。

### 3. 保持简洁

优化后的 CLAUDE.md 应包含：

```
# 项目是什么
## 核心功能/工作流
## Key Patterns（组件概览）
## 重要规则（自己关心的）
## 避免什么
## 详细规格引用（指向 Rules）
```

## 优化前后对比

| 指标 | 优化前 | 优化后 |
|------|--------|--------|
| 行数 | 126 行 | 67 行 |
| 技术细节 | 内联在文件中 | 分离到 Rules |
| 职责 | 项目记忆 + 技术文档 | 仅项目记忆 |

## 示例

优化前（126行）：
```
# CLAUDE.md
## Repository Overview
### Weather System (Example Workflow)
  - /weather-orchestrator command...
  - weather-agent agent...
  - weather-fetcher skill...
### Skill Definition Structure
  - name: Display name and /slash-command...
  - description: When to invoke...
  [... 10+ 字段详细说明 ...]
```

优化后（67行）：
```
# CLAUDE.md
## What This Repo Does
Demonstrates Claude Code patterns...
### Weather Workflow (Command → Agent → Skill)
/weather-orchestrator → weather-agent → weather-fetcher skill + weather-svg-creator skill
## Key Patterns
- Commands for workflows (.claude/commands/*.md)
- Agents for specialist roles (.claude/agents/*.md)
- Skills for reusable instructions (.claude/skills/*/SKILL.md)
## Detailed Specs (Rules)
For technical field references, see:
- .claude/rules/skill-fields.md — SKILL.md YAML fields
- .claude/rules/agent-fields.md — Agent YAML fields
```

## 关键规则

1. **CLAUDE.md ≤ 200 行** — 超过则拆入 Rules
2. **技术规格不放 CLAUDE.md** — 放 Rules 按需加载
3. **Rules 支持 Glob 模式** — 只在相关文件被访问时加载
4. **CLAUDE.md 回答"做什么"** — 不是"怎么做"
