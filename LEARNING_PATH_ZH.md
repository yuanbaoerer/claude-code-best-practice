# Claude Code 最佳实践学习路径

基于 [claude-code-best-practice](https://github.com/shanraisshan/claude-code-best-practice) 仓库整理的系统学习指南。

---

## 🟢 第一阶段：基础入门（1-2 天）

### 1. 安装与环境

- [tutorial/day0/README_ZH.md](tutorial/day0/README_ZH.md) - 安装概览
- 根据你的操作系统选择：
  - [tutorial/day0/mac_ZH.md](tutorial/day0/mac_ZH.md)
  - [tutorial/day0/linux_ZH.md](tutorial/day0/linux_ZH.md)
  - [tutorial/day0/windows_ZH.md](tutorial/day0/windows_ZH.md)

### 2. 核心概念理解

阅读 [README_ZH.md](README_ZH.md) 的 **🧠 概念** 表格，了解：

| 概念 | 说明 |
|------|------|
| <img src="!/tags/a.svg" height="14"> **子代理 (Subagents)** | 独立上下文的自主执行者 — 自定义工具、权限、模型、记忆 |
| <img src="!/tags/c.svg" height="14"> **命令 (Commands)** | 用户调用的提示模板 — 工作流编排入口点 |
| <img src="!/tags/s.svg" height="14"> **技能 (Skills)** | 可自动发现的知识注入 — 可预加载、可复用 |
| **钩子 (Hooks)** | 事件触发的处理程序 — 在代理循环之外执行 |
| **MCP 服务器** | 外部工具连接 — 数据库、API、工具集成 |
| **设置 (Settings)** | 分层配置系统 — 权限、模型、输出风格 |
| **记忆 (Memory)** | 持久上下文 — CLAUDE.md 文件和规则 |

---

## 🟡 第二阶段：最佳实践深入（3-5 天）

按重要性顺序阅读：

| 顺序 | 文档 | 学习目标 |
|:---:|------|---------|
| 1 | [best-practice/claude-memory_ZH.md](best-practice/claude-memory_ZH.md) | 学会编写有效的 CLAUDE.md |
| 2 | [best-practice/claude-subagents_ZH.md](best-practice/claude-subagents_ZH.md) | 创建自定义子代理 |
| 3 | [best-practice/claude-skills_ZH.md](best-practice/claude-skills_ZH.md) | 定义可复用技能 |
| 4 | [best-practice/claude-commands_ZH.md](best-practice/claude-commands_ZH.md) | 编写斜杠命令 |
| 5 | [best-practice/claude-settings_ZH.md](best-practice/claude-settings_ZH.md) | 配置权限和模型 |
| 6 | [best-practice/claude-mcp_ZH.md](best-practice/claude-mcp_ZH.md) | 连接外部工具 |
| 7 | [best-practice/claude-cli-startup-flags_ZH.md](best-practice/claude-cli-startup-flags_ZH.md) | CLI 启动参数 |

---

## 🟠 第三阶段：架构模式（2-3 天）

### 1. 编排工作流 — 核心架构模式

- [orchestration-workflow/orchestration-workflow_ZH.md](orchestration-workflow/orchestration-workflow_ZH.md)
- 理解 **命令 → 代理 → 技能** 分层编排
- 实际演示：`/weather-orchestrator`

### 2. 三种机制对比 — 何时用什么

- [reports/claude-agent-command-skill_ZH.md](reports/claude-agent-command-skill_ZH.md)
- 理解自动调用、上下文隔离、用户可见性的区别

| 机制 | 自动调用 | 上下文隔离 | 用户可见 |
|------|:--------:|:----------:|:--------:|
| 代理 | ✅ 通过 description | ✅ 始终独立 | ❌ `/` 菜单不显示 |
| 命令 | ❌ 仅用户发起 | ❌ 共享主窗口 | ✅ `/command-name` |
| 技能 | ✅ 通过 description | ⚙️ 可选 (`context: fork`) | ✅ 默认显示 |

### 3. 代理记忆系统

- [reports/claude-agent-memory_ZH.md](reports/claude-agent-memory_ZH.md)
- 让代理跨会话学习和积累知识

| 范围 | 存储位置 | 最佳用途 |
|------|----------|---------|
| `user` | `~/.claude/agent-memory/<agent>/` | 跨项目知识（推荐默认） |
| `project` | `.claude/agent-memory/<agent>/` | 团队共享的项目知识 |
| `local` | `.claude/agent-memory-local/<agent>/` | 个人项目特定知识 |

---

## 🔴 第四阶段：开发工作流（3-5 天）

### 推荐顺序

| 工作流 | 特点 | 适合场景 |
|--------|------|---------|
| [RPI 工作流](development-workflows/rpi/rpi-workflow_ZH.md) | 研究→计划→实现，每阶段有验证关卡 | 新功能开发，防止不可行设计 |
| [跨模型工作流](development-workflows/cross-model-workflow/cross-model-workflow_ZH.md) | Claude Code + Codex 交替审查 | 高质量代码，双重验证 |
| [Superpowers](https://github.com/obra/superpowers) | TDD 优先，铁律约束 | 严格测试驱动开发 |
| [BMAD-METHOD](https://github.com/bmad-code-org/BMAD-METHOD) | 完整 SDLC，22+ 平台 | 全生命周期管理 |

### RPI 工作流详解

```
RPI = Research → Plan → Implement

步骤 1: 研究阶段
/rpi:research → GO/NO-GO 分析

步骤 2: 计划阶段
/rpi:plan → 用户故事 + 技术架构

步骤 3: 实现阶段
/rpi:implement → 分阶段执行 + 测试关卡
```

---

## 🟣 第五阶段：实战技巧（持续学习）

### Boris Cherny（Claude Code 创造者）技巧系列

| 文档 | 内容 |
|------|------|
| [tips/claude-boris-15-tips-30-mar-26_ZH.md](tips/claude-boris-15-tips-30-mar-26_ZH.md) | 最新 15 条隐藏功能 |
| [tips/claude-boris-13-tips-03-jan-26_ZH.md](tips/claude-boris-13-tips-03-jan-26_ZH.md) | 13 条核心技巧 |
| [tips/claude-boris-10-tips-01-feb-26_ZH.md](tips/claude-boris-10-tips-01-feb-26_ZH.md) | 10 条进阶技巧 |
| [tips/claude-boris-12-tips-12-feb-26_ZH.md](tips/claude-boris-12-tips-12-feb-26_ZH.md) | 12 条实践技巧 |

### 关键技巧摘要

| 类别 | 技巧 |
|------|------|
| **规划** | 始终从计划模式开始 |
| **CLAUDE.md** | 保持 200 行以内 |
| **规范** | 让 Claude 用 AskUserQuestion 对你访谈 |
| **执行** | 分阶段把关计划，每阶段有测试 |
| **干预** | 不要过度管理 — "修复" 比 "怎么做" 更好 |
| **审查** | 挑战 Claude — "严厉审查直到通过测试" |
| **重构** | 平庸修复后 — "废弃这个，实现优雅方案" |

---

## 🔵 第六阶段：高级主题（选修）

| 文档 | 主题 |
|------|------|
| [reports/claude-advanced-tool-use_ZH.md](reports/claude-advanced-tool-use_ZH.md) | 高级工具使用模式 |
| [reports/claude-skills-for-larger-mono-repos_ZH.md](reports/claude-skills-for-larger-mono-repos_ZH.md) | 大型 monorepo 技能策略 |
| [reports/claude-global-vs-project-settings_ZH.md](reports/claude-global-vs-project-settings_ZH.md) | 设置层级深度解析 |
| [reports/claude-agent-sdk-vs-cli-system-prompts_ZH.md](reports/claude-agent-sdk-vs-cli-system-prompts_ZH.md) | SDK vs CLI 系统提示对比 |
| [reports/claude-in-chrome-v-chrome-devtools-mcp_ZH.md](reports/claude-in-chrome-v-chrome-devtools-mcp_ZH.md) | 浏览器自动化方案对比 |
| [reports/claude-usage-and-rate-limits_ZH.md](reports/claude-usage-and-rate-limits_ZH.md) | 使用量和速率限制 |
| [reports/llm-day-to-day-degradation_ZH.md](reports/llm-day-to-day-degradation_ZH.md) | LLM 日常退化分析 |

---

## 📺 视频学习（补充）

| 视频 | 内容 |
|------|------|
| [videos/claude-boris-lennys-podcast-19-feb-26_ZH.md](videos/claude-boris-lennys-podcast-19-feb-26_ZH.md) | Lenny's Podcast 深度访谈 |
| [videos/claude-boris-pragmatic-engineer-04-mar-26_ZH.md](videos/claude-boris-pragmatic-engineer-04-mar-26_ZH.md) | Pragmatic Engineer 对话 |
| [videos/claude-boris-y-combinator-17-feb-26_ZH.md](videos/claude-boris-y-combinator-17-feb-26_ZH.md) | Y Combinator 演讲 |
| [videos/claude-boris-ryan-peterman-15-dec-25_ZH.md](videos/claude-boris-ryan-peterman-15-dec-25_ZH.md) | Ryan Peterman 对话 |
| [videos/claude-cat-every-29-oct-25_ZH.md](videos/claude-cat-every-29-oct-25_ZH.md) | Cat 的全面演示 |

---

## 🛠️ 实现示例参考

本仓库的 `.claude/` 目录包含可运行的示例：

| 目录 | 内容 |
|------|------|
| `.claude/agents/` | 子代理定义（如 `weather-agent.md`） |
| `.claude/commands/` | 斜杠命令（如 `weather-orchestrator.md`） |
| `.claude/skills/` | 技能定义（如 `weather-fetcher/`） |
| `.claude/hooks/` | 钩子脚本和配置 |
| `.claude/settings.json` | 设置配置示例 |
| `CLAUDE.md` | 项目指令示例 |

---

## 📖 学习建议

1. **边学边做** — 在自己的项目中尝试每个概念
2. **从简单开始** — 先写 CLAUDE.md，再创建简单技能
3. **参考实现** — 本仓库是可运行的完整示例
4. **关注技巧** — Boris 的技巧来自实际经验，价值最高
5. **工作流选一** — 不需要学所有工作流，选一个适合你的深入
6. **手动 compact** — 上下文约 50% 时执行 `/compact`

---

## ⏱️ 学习时间预估

| 阶段 | 时间 | 目标 |
|------|------|------|
| 基础入门 | 1-2 天 | 安装并理解核心概念 |
| 最佳实践 | 3-5 天 | 掌握 CLAUDE.md、代理、技能、命令 |
| 架构模式 | 2-3 天 | 理解编排工作流和机制对比 |
| 开发工作流 | 3-5 天 | 选定并实践一种工作流 |
| 实战技巧 | 持续 | 学习并应用技巧 |
| 高级主题 | 选修 | 根据需要深入学习 |

**总计**：10-15 天掌握核心内容，持续实践深化理解。

---

<table width="100%">
<tr>
<td><a href="./">← 返回 Claude Code 最佳实践</a></td>
<td align="right"><img src="!/claude-jumping.svg" alt="Claude" width="60" /></td>
</tr>
</table>