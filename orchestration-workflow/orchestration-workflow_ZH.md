# 编排工作流程

本文档描述 **命令 → 代理（带技能） → 技能** 编排工作流程，通过天气数据获取和 SVG 渲染系统进行演示。

<table width="100%">
<tr>
<td><a href="../">← 返回 Claude Code 最佳实践</a></td>
<td align="right"><img src="../!/claude-jumping.svg" alt="Claude" width="60" /></td>
</tr>
</table>

## 系统概述

天气系统在单个编排工作流程中演示了两种不同的技能模式：
- **代理技能**（预加载）：`weather-fetcher` 在启动时注入到 `weather-agent` 作为领域知识
- **技能**（独立）：`weather-svg-creator` 由命令通过 Skill 工具直接调用

这展示了 **命令 → 代理 → 技能** 架构模式，其中：
- 命令编排工作流程并处理用户交互
- 代理使用其预加载技能获取数据
- 技能独立创建视觉输出

## 组件摘要

| 组件 | 角色 | 示例 |
|------|------|------|
| **命令** | 入口点，用户交互 | [`/weather-orchestrator`](../.claude/commands/weather-orchestrator.md) |
| **代理** | 使用预加载技能（代理技能）获取数据 | [`weather-agent`](../.claude/agents/weather-agent.md) 与 [`weather-fetcher`](../.claude/skills/weather-fetcher/SKILL.md) |
| **技能** | 独立创建输出（技能） | [`weather-svg-creator`](../.claude/skills/weather-svg-creator/SKILL.md) |

## 流程图

```
╔══════════════════════════════════════════════════════════════════╗
║              编排工作流程                                          ║
║           命令  →  代理  →  技能                                   ║
╚══════════════════════════════════════════════════════════════════╝

                         ┌───────────────────┐
                         │    用户交互       │
                         └─────────┬─────────┘
                                   │
                                   ▼
         ┌─────────────────────────────────────────────────────┐
         │  /weather-orchestrator — 命令（入口点）              │
         └─────────────────────────┬───────────────────────────┘
                                   │
                              步骤 1
                                   │
                                   ▼
                      ┌────────────────────────┐
                      │  AskUser — C° 或 F°?   │
                      └────────────┬───────────┘
                                   │
                         步骤 2 — Agent 工具
                                   │
                                   ▼
         ┌─────────────────────────────────────────────────────┐
         │  weather-agent — 代理 ● 技能: weather-fetcher       │
         └─────────────────────────┬───────────────────────────┘
                                   │
                          返回: 温度 + 单位
                                   │
                         步骤 3 — Skill 工具
                                   │
                                   ▼
         ┌─────────────────────────────────────────────────────┐
         │  weather-svg-creator — 技能 ● SVG 卡片 + output     │
         └─────────────────────────┬───────────────────────────┘
                                   │
                          ┌────────┴────────┐
                          │                 │
                          ▼                 ▼
                   ┌────────────┐    ┌────────────┐
                   │weather.svg │    │ output.md  │
                   └────────────┘    └────────────┘
```

## 组件详情

### 1. 命令

#### `/weather-orchestrator`（命令）
- **位置**：`.claude/commands/weather-orchestrator.md`
- **用途**：入口点 —— 编排工作流程并处理用户交互
- **动作**：
  1. 询问用户温度单位偏好（摄氏/华氏）
  2. 通过 Agent 工具调用 weather-agent
  3. 通过 Skill 工具调用 weather-svg-creator
- **模型**：haiku

### 2. 带预加载技能的代理（代理技能）

#### `weather-agent`（代理）
- **位置**：`.claude/agents/weather-agent.md`
- **用途**：使用其预加载技能获取天气数据
- **技能**：`weather-fetcher`（预加载作为领域知识）
- **可用工具**：WebFetch, Read
- **模型**：sonnet
- **颜色**：绿色
- **内存**：项目

代理在启动时将 `weather-fetcher` 预加载到其上下文中。它遵循技能的指令获取温度并将值返回给命令。

### 3. 技能

#### `weather-svg-creator`（技能）
- **位置**：`.claude/skills/weather-svg-creator/SKILL.md`
- **用途**：创建视觉 SVG 天气卡片并写入输出文件
- **调用方式**：通过命令的 Skill 工具（不预加载到任何代理）
- **输出**：
  - `orchestration-workflow/weather.svg` — SVG 天气卡片
  - `orchestration-workflow/output.md` — 天气摘要

### 4. 预加载技能

#### `weather-fetcher`（技能）
- **位置**：`.claude/skills/weather-fetcher/SKILL.md`
- **用途**：获取实时温度数据的指令
- **数据源**：Open-Meteo API，迪拜，阿联酋
- **输出**：温度值和单位（摄氏或华氏）
- **注意**：这是一个代理技能 —— 预加载到 `weather-agent`，不直接调用

## 执行流程

1. **用户调用**：用户运行 `/weather-orchestrator` 命令
2. **用户提示**：命令询问用户偏好的温度单位（摄氏/华氏）
3. **代理调用**：命令通过 Agent 工具调用 `weather-agent`
4. **技能执行**（在代理上下文内）：
   - 代理遵循 `weather-fetcher` 技能指令从 Open-Meteo 获取温度
   - 代理将温度值和单位返回给命令
5. **SVG 创建**：命令通过 Skill 工具调用 `weather-svg-creator`
   - 技能在 `orchestration-workflow/weather.svg` 创建 SVG 天气卡片
   - 技能将摘要写入 `orchestration-workflow/output.md`
6. **结果展示**：向用户显示摘要，包含：
   - 请求的温度单位
   - 获取的温度
   - SVG 卡片位置
   - 输出文件位置

## 示例执行

```
输入: /weather-orchestrator
├─ 步骤 1: 询问: 摄氏还是华氏？
│  └─ 用户: 摄氏
├─ 步骤 2: Agent 工具 → weather-agent
│  ├─ 预加载技能:
│  │  └─ weather-fetcher（领域知识）
│  ├─ 从 Open-Meteo 获取 → 26°C
│  └─ 返回: temperature=26, unit=Celsius
├─ 步骤 3: Skill 工具 → /weather-svg-creator
│  ├─ 创建: orchestration-workflow/weather.svg
│  └─ 写入: orchestration-workflow/output.md
└─ 输出:
   ├─ 单位: 摄氏
   ├─ 温度: 26°C
   ├─ SVG: orchestration-workflow/weather.svg
   └─ 摘要: orchestration-workflow/output.md
```

## 关键设计原则

1. **两种技能模式**：演示代理技能（预加载）和技能（直接调用）
2. **命令作为编排者**：命令处理用户交互并协调工作流程
3. **代理用于数据获取**：代理使用其预加载技能获取数据，然后返回它
4. **技能用于输出**：SVG 创建器独立运行，从命令上下文接收数据
5. **清晰分离**：获取（代理）→ 渲染（技能）—— 每个组件有单一职责

## 架构模式

### 代理技能（预加载）

```yaml
# 在代理定义中 (.claude/agents/weather-agent.md)
---
name: weather-agent
skills:
  - weather-fetcher    # 启动时预加载到代理上下文
---
```

- **技能被预加载**：完整技能内容在启动时注入到代理上下文
- **代理使用技能知识**：代理遵循预加载技能的指令
- **无动态调用**：技能是参考材料，不单独调用

### 技能（直接调用）

```yaml
# 在技能定义中 (.claude/skills/weather-svg-creator/SKILL.md)
---
name: weather-svg-creator
description: 创建 SVG 天气卡片...
---
```

- **通过 Skill 工具调用**：命令调用 `Skill(skill: "weather-svg-creator")`
- **独立执行**：在命令上下文中运行，不在代理内
- **从上下文接收数据**：使用对话中已有的温度数据