# claude-code-best-practice
熟能生巧，让 Claude 更完美

![使用 Claude Code 更新](https://img.shields.io/badge/updated_with_Claude_Code-v2.1.87%20(Mar%2030%2C%202026%2011%3A44%20AM%20PKT)-white?style=flat&labelColor=555) <a href="https://github.com/shanraisshan/claude-code-best-practice/stargazers"><img src="https://img.shields.io/github/stars/shanraisshan/claude-code-best-practice?style=flat&label=%E2%98%85&labelColor=555&color=white" alt="GitHub Stars"></a><br>

[![最佳实践](!/tags/best-practice.svg)](best-practice/) [![已实现](!/tags/implemented.svg)](implementation/) [![编排工作流](!/tags/orchestration-workflow.svg)](orchestration-workflow/orchestration-workflow.md) [![Boris](!/tags/boris-cherny.svg)](#-tips-and-tricks) ![点击下方徽章查看实际来源](!/tags/click-badges.svg)<br>
<img src="!/tags/a.svg" height="14"> = 子代理 · <img src="!/tags/c.svg" height="14"> = 命令 · <img src="!/tags/s.svg" height="14"> = 技能

<p align="center">
  <img src="!/claude-jumping.svg" alt="Claude Code 吉祥物跳跃" width="120" height="100"><br>
  <a href="https://github.com/trending"><img src="!/root/github-trending-day.svg" alt="GitHub 当日热门仓库 #1"></a>
</p>

<p align="center">
  <img src="!/root/boris-slider.gif" alt="Boris Cherny 谈 Claude Code" width="600"><br>
  Boris Cherny 在 X 平台 (<a href="https://x.com/bcherny/status/2007179832300581177">推文 1</a> · <a href="https://x.com/bcherny/status/2017742741636321619">推文 2</a> · <a href="https://x.com/bcherny/status/2021699851499798911">推文 3</a>)
</p>


## 🧠 概念

| 功能 | 位置 | 描述 |
|---------|----------|-------------|
| <img src="!/tags/a.svg" height="14"> [**子代理 (Subagents)**](https://code.claude.com/docs/en/sub-agents) | `.claude/agents/<name>.md` | [![最佳实践](!/tags/best-practice.svg)](best-practice/claude-subagents.md) [![已实现](!/tags/implemented.svg)](implementation/claude-subagents-implementation.md) 在全新隔离上下文中的自主执行者 — 自定义工具、权限、模型、记忆和持久身份 |
| <img src="!/tags/c.svg" height="14"> [**命令 (Commands)**](https://code.claude.com/docs/en/slash-commands) | `.claude/commands/<name>.md` | [![最佳实践](!/tags/best-practice.svg)](best-practice/claude-commands.md) [![已实现](!/tags/implemented.svg)](implementation/claude-commands-implementation.md) 注入到现有上下文的知识 — 简单的用户调用提示模板，用于工作流编排 |
| <img src="!/tags/s.svg" height="14"> [**技能 (Skills)**](https://code.claude.com/docs/en/skills) | `.claude/skills/<name>/SKILL.md` | [![最佳实践](!/tags/best-practice.svg)](best-practice/claude-skills.md) [![已实现](!/tags/implemented.svg)](implementation/claude-skills-implementation.md) 注入到现有上下文的知识 — 可配置、可预加载、可自动发现，支持上下文分叉和渐进式披露 · [官方技能](https://github.com/anthropics/skills/tree/main/skills) |
| [**工作流 (Workflows)**](https://code.claude.com/docs/en/common-workflows) | [`.claude/commands/weather-orchestrator.md`](.claude/commands/weather-orchestrator.md) | [![编排工作流](!/tags/orchestration-workflow.svg)](orchestration-workflow/orchestration-workflow.md) |
| [**钩子 (Hooks)**](https://code.claude.com/docs/en/hooks) | `.claude/hooks/` | [![最佳实践](!/tags/best-practice.svg)](https://github.com/shanraisshan/claude-code-hooks) [![已实现](!/tags/implemented.svg)](https://github.com/shanraisshan/claude-code-hooks) 在特定事件时运行的用户定义处理程序（脚本、HTTP、提示、代理），在代理循环之外执行 · [指南](https://code.claude.com/docs/en/hooks-guide) |
| [**MCP 服务器**](https://code.claude.com/docs/en/mcp) | `.claude/settings.json`, `.mcp.json` | [![最佳实践](!/tags/best-practice.svg)](best-practice/claude-mcp.md) [![已实现](!/tags/implemented.svg)](.mcp.json) 模型上下文协议连接外部工具、数据库和 API |
| [**插件 (Plugins)**](https://code.claude.com/docs/en/plugins) | 可分发包 | 技能、子代理、钩子、MCP 服务器和 LSP 服务器的打包组合 · [市场](https://code.claude.com/docs/en/discover-plugins) · [创建市场](https://code.claude.com/docs/en/plugin-marketplaces) |
| [**设置 (Settings)**](https://code.claude.com/docs/en/settings) | `.claude/settings.json` | [![最佳实践](!/tags/best-practice.svg)](best-practice/claude-settings.md) [![已实现](!/tags/implemented.svg)](.claude/settings.json) 分层配置系统 · [权限](https://code.claude.com/docs/en/permissions) · [模型配置](https://code.claude.com/docs/en/model-config) · [输出风格](https://code.claude.com/docs/en/output-styles) · [沙箱](https://code.claude.com/docs/en/sandboxing) · [键绑定](https://code.claude.com/docs/en/keybindings) · [快速模式](https://code.claude.com/docs/en/fast-mode) |
| [**状态栏 (Status Line)**](https://code.claude.com/docs/en/statusline) | `.claude/settings.json` | [![最佳实践](!/tags/best-practice.svg)](https://github.com/shanraisshan/claude-code-status-line) [![已实现](!/tags/implemented.svg)](.claude/settings.json) 显示上下文使用量、模型、成本和会话信息的可定制状态栏 |
| [**记忆 (Memory)**](https://code.claude.com/docs/en/memory) | `CLAUDE.md`, `.claude/rules/`, `~/.claude/rules/`, `~/.claude/projects/<project>/memory/` | [![最佳实践](!/tags/best-practice.svg)](best-practice/claude-memory.md) [![已实现](!/tags/implemented.svg)](CLAUDE.md) 通过 CLAUDE.md 文件和 `@path` 导入实现持久上下文 · [自动记忆](https://code.claude.com/docs/en/memory) · [规则](https://code.claude.com/docs/en/memory#organize-rules-with-clauderules) |
| [**检查点 (Checkpointing)**](https://code.claude.com/docs/en/checkpointing) | 自动（基于 git） | 自动跟踪文件编辑，支持回退（`Esc Esc` 或 `/rewind`）和针对性摘要 |
| [**CLI 启动参数**](https://code.claude.com/docs/en/cli-reference) | `claude [flags]` | [![最佳实践](!/tags/best-practice.svg)](best-practice/claude-cli-startup-flags.md) 启动 Claude Code 的命令行参数、子命令和环境变量 · [交互模式](https://code.claude.com/docs/en/interactive-mode) |
| **AI 术语** | | [![最佳实践](!/tags/best-practice.svg)](https://github.com/shanraisshan/claude-code-codex-cursor-gemini/blob/main/reports/ai-terms.md) 代理工程 · 上下文工程 · 氛围编码 |
| [**最佳实践**](https://code.claude.com/docs/en/best-practices) | | 官方最佳实践 · [提示工程](https://github.com/anthropics/prompt-eng-interactive-tutorial) · [扩展 Claude Code](https://code.claude.com/docs/en/features-overview) |

### 🔥 热门

| 功能 | 位置 | 描述 |
|---------|----------|-------------|
| [**自动模式 (Auto Mode)**](https://code.claude.com/docs/en/permission-modes#eliminate-prompts-with-auto-mode) ![beta](!/tags/beta.svg) | `--permission-mode auto` | [![最佳实践](!/tags/best-practice.svg)](https://x.com/claudeai/status/2036503582166393240) 后台安全分类器替代手动权限提示 — Claude 决定什么是安全的，同时阻止提示注入和风险升级 · [博客](https://claude.com/blog/auto-mode) |
| [**频道 (Channels)**](https://code.claude.com/docs/en/channels) ![beta](!/tags/beta.svg) | `--channels`, 基于插件 | 从 Telegram、Discord 或 webhook 推送事件到运行中的会话 — Claude 在你离开时也能响应 · [参考](https://code.claude.com/docs/en/channels-reference) |
| [**Slack**](https://code.claude.com/docs/en/slack) | Slack 中 `@Claude` | 在团队聊天中提及 @Claude 并提出编码任务 — 路由到 Claude Code 网页会话进行 bug 修复、代码审查和并行任务执行 |
| [**代码审查 (Code Review)**](https://code.claude.com/docs/en/code-review) ![beta](!/tags/beta.svg) | GitHub App（托管） | [![最佳实践](!/tags/best-practice.svg)](https://x.com/claudeai/status/2031088171262554195) 多代理 PR 分析，捕捉 bug、安全漏洞和回归问题 · [博客](https://claude.com/blog/code-review) |
| [**GitHub Actions**](https://code.claude.com/docs/en/github-actions) | `.github/workflows/` | 在 CI/CD 管道中自动化 PR 审查、问题分类和代码生成 · [GitLab CI/CD](https://code.claude.com/docs/en/gitlab-ci-cd) |
| [**Chrome**](https://code.claude.com/docs/en/chrome) ![beta](!/tags/beta.svg) | `--chrome`, 扩展 | [![最佳实践](!/tags/best-practice.svg)](reports/claude-in-chrome-v-chrome-devtools-mcp.md) 通过 Claude in Chrome 实现浏览器自动化 — 测试 web 应用、使用控制台调试、自动化表单、从页面提取数据 |
| [**计划任务 (Scheduled Tasks)**](https://code.claude.com/docs/en/scheduled-tasks) | `/loop`, `/schedule`, cron 工具 | [![最佳实践](!/tags/best-practice.svg)](https://x.com/bcherny/status/2030193932404150413) [![已实现](!/tags/implemented.svg)](implementation/claude-scheduled-tasks-implementation.md) `/loop` 在本地按循环计划运行提示（最多 3 天）· [`/schedule`](https://code.claude.com/docs/en/web-scheduled-tasks) 在 Anthropic 云基础设施上运行提示 — 即使你的机器关闭也能工作 · [公告](https://x.com/noahzweben/status/2036129220959805859) |
| [**语音听写 (Voice Dictation)**](https://code.claude.com/docs/en/voice-dictation) ![beta](!/tags/beta.svg) | `/voice` | [![最佳实践](!/tags/best-practice.svg)](https://x.com/trq212/status/2028628570692890800) 按键说话的语音输入，支持 20 种语言和可重新绑定的激活键 |
| [**简化与批处理 (Simplify & Batch)**](https://code.claude.com/docs/en/skills#bundled-skills) | `/simplify`, `/batch` | [![最佳实践](!/tags/best-practice.svg)](https://x.com/bcherny/status/2027534984534544489) 内置技能用于代码质量和批量操作 — simplify 重构以提高复用性和效率，batch 跨文件运行命令 |
| [**代理团队 (Agent Teams)**](https://code.claude.com/docs/en/agent-teams) ![beta](!/tags/beta.svg) | 内置（环境变量） | [![最佳实践](!/tags/best-practice.svg)](https://x.com/bcherny/status/2019472394696683904) [![已实现](!/tags/implemented.svg)](implementation/claude-agent-teams-implementation.md) 多个代理在同一代码库上并行工作，共享任务协调 |
| [**远程控制 (Remote Control)**](https://code.claude.com/docs/en/remote-control) | `/remote-control`, `/rc` | [![最佳实践](!/tags/best-practice.svg)](https://x.com/noahzweben/status/2032533699116355819) 从任何设备继续本地会话 — 手机、平板或浏览器 · [无头模式](https://code.claude.com/docs/en/headless) |
| [**Git Worktrees**](https://code.claude.com/docs/en/common-workflows#run-parallel-claude-code-sessions-with-git-worktrees) | 内置 | [![最佳实践](!/tags/best-practice.svg)](https://x.com/bcherny/status/2025007393290272904) 用于并行开发的隔离 git 分支 — 每个代理有自己的工作副本 |
| [**Ralph Wiggum Loop**](https://github.com/anthropics/claude-code/tree/main/plugins/ralph-wiggum) | 插件 | [![最佳实践](!/tags/best-practice.svg)](https://github.com/ghuntley/how-to-ralph-wiggum) [![已实现](!/tags/implemented.svg)](https://github.com/shanraisshan/novel-llm-26) 用于长时间运行任务的自主开发循环 — 迭代直到完成 |

<p align="center">
  <img src="!/claude-jumping.svg" alt="section divider" width="60" height="50">
</p>

<a id="orchestration-workflow"></a>

## <a href="orchestration-workflow/orchestration-workflow.md"><img src="!/tags/orchestration-workflow-hd.svg" alt="编排工作流"></a>

详见 [orchestration-workflow](orchestration-workflow/orchestration-workflow.md) 了解 <img src="!/tags/c.svg" height="14"> **命令** → <img src="!/tags/a.svg" height="14"> **代理** → <img src="!/tags/s.svg" height="14"> **技能** 模式的实现细节。


<p align="center">
  <img src="orchestration-workflow/orchestration-workflow.svg" alt="命令技能代理架构流程" width="100%">
</p>

<p align="center">
  <img src="orchestration-workflow/orchestration-workflow.gif" alt="编排工作流演示" width="600">
</p>

![如何使用](!/tags/how-to-use.svg)

```bash
claude
/weather-orchestrator
```

<p align="center">
  <img src="!/claude-jumping.svg" alt="section divider" width="60" height="50">
</p>

## ⚙️ 开发工作流

所有主流工作流都汇聚到相同的架构模式：**研究 → 计划 → 执行 → 审查 → 发布**

| 名称 | ★ | 独特性 | 计划 | <img src="!/tags/a.svg" height="14"> | <img src="!/tags/c.svg" height="14"> | <img src="!/tags/s.svg" height="14"> |
|------|---|------------|------|---|---|---|
| [Superpowers](https://github.com/obra/superpowers) | 122k | ![TDD优先](https://img.shields.io/badge/TDD--first-ddf4ff) ![铁律](https://img.shields.io/badge/Iron_Laws-ddf4ff) ![全计划审查](https://img.shields.io/badge/whole--plan_review-ddf4ff) | <img src="!/tags/s.svg" height="14"> [writing-plans](https://github.com/obra/superpowers/tree/main/skills/writing-plans) | 5 | 3 | 14 |
| [Everything Claude Code](https://github.com/affaan-m/everything-claude-code) | 116k | ![直觉评分](https://img.shields.io/badge/instinct_scoring-ddf4ff) ![AgentShield](https://img.shields.io/badge/AgentShield-ddf4ff) ![多语言规则](https://img.shields.io/badge/multi--lang_rules-ddf4ff) | <img src="!/tags/a.svg" height="14"> [planner](https://github.com/affaan-m/everything-claude-code/blob/main/agents/planner.md) | 30 | 63 | 135 |
| [Spec Kit](https://github.com/github/spec-kit) | 83k | ![规范驱动](https://img.shields.io/badge/spec--driven-ddf4ff) ![宪法](https://img.shields.io/badge/constitution-ddf4ff) ![22+ 工具](https://img.shields.io/badge/22%2B_tools-ddf4ff) | <img src="!/tags/c.svg" height="14"> [speckit.plan](https://github.com/github/spec-kit/blob/main/templates/commands/plan.md) | 0 | 9+ | 0 |
| [gstack](https://github.com/garrytan/gstack) | 55k | ![角色人格](https://img.shields.io/badge/role_personas-ddf4ff) ![/codex review](https://img.shields.io/badge/%2Fcodex_review-ddf4ff) ![并行冲刺](https://img.shields.io/badge/parallel_sprints-ddf4ff) | <img src="!/tags/s.svg" height="14"> [autoplan](https://github.com/garrytan/gstack/tree/main/autoplan) | 0 | 0 | 28 |
| [Get Shit Done](https://github.com/gsd-build/get-shit-done) | 44k | ![全新 200K 上下文](https://img.shields.io/badge/fresh_200K_contexts-ddf4ff) ![波次执行](https://img.shields.io/badge/wave_execution-ddf4ff) ![XML 计划](https://img.shields.io/badge/XML_plans-ddf4ff) | <img src="!/tags/a.svg" height="14"> [gsd-planner](https://github.com/gsd-build/get-shit-done/blob/main/agents/gsd-planner.md) | 18 | 57 | 0 |
| [BMAD-METHOD](https://github.com/bmad-code-org/BMAD-METHOD) | 43k | ![完整 SDLC](https://img.shields.io/badge/full_SDLC-ddf4ff) ![代理人格](https://img.shields.io/badge/agent_personas-ddf4ff) ![22+ 平台](https://img.shields.io/badge/22%2B_platforms-ddf4ff) | <img src="!/tags/s.svg" height="14"> [bmad-create-prd](https://github.com/bmad-code-org/BMAD-METHOD/tree/main/src/bmm-skills/2-plan-workflows/bmad-create-prd) | 0 | 0 | 40 |
| [OpenSpec](https://github.com/Fission-AI/OpenSpec) | 35k | ![增量规范](https://img.shields.io/badge/delta_specs-ddf4ff) ![棕地开发](https://img.shields.io/badge/brownfield-ddf4ff) ![工件 DAG](https://img.shields.io/badge/artifact_DAG-ddf4ff) | <img src="!/tags/c.svg" height="14"> [opsx:propose](https://github.com/Fission-AI/OpenSpec/blob/main/src/commands/workflow/new-change.ts) | 0 | 11 | 0 |
| [Compound Engineering](https://github.com/EveryInc/compound-engineering-plugin) | 12k | ![复合学习](https://img.shields.io/badge/Compound_Learning-ddf4ff) ![多平台 CLI](https://img.shields.io/badge/Multi--Platform_CLI-ddf4ff) ![插件市场](https://img.shields.io/badge/Plugin_Marketplace-ddf4ff) | <img src="!/tags/s.svg" height="14"> [ce-plan](https://github.com/EveryInc/compound-engineering-plugin/tree/main/plugins/compound-engineering/skills/ce-plan) | 48 | 4 | 42 |
| [HumanLayer](https://github.com/humanlayer/humanlayer) | 10k | ![RPI](https://img.shields.io/badge/RPI-ddf4ff) ![上下文工程](https://img.shields.io/badge/context_engineering-ddf4ff) ![300k+ LOC](https://img.shields.io/badge/300k%2B_LOC-ddf4ff) | <img src="!/tags/c.svg" height="14"> [create_plan](https://github.com/humanlayer/humanlayer/blob/main/.claude/commands/create_plan.md) | 6 | 27 | 0 |

### 其他
- [跨模型（Claude Code + Codex）工作流](development-workflows/cross-model-workflow/cross-model-workflow.md) [![已实现](!/tags/implemented.svg)](development-workflows/cross-model-workflow/cross-model-workflow.md)
- [RPI](development-workflows/rpi/rpi-workflow.md) [![已实现](!/tags/implemented.svg)](development-workflows/rpi/rpi-workflow.md)
- [Ralph Wiggum Loop](https://www.youtube.com/watch?v=eAtvoGlpeRU) [![已实现](!/tags/implemented.svg)](https://github.com/shanraisshan/novel-llm-26)
- [Andrej Karpathy（OpenAI 创始成员）工作流](https://x.com/karpathy/status/2015883857489522876)
- [Peter Steinberger（OpenClaw 创造者）工作流](https://youtu.be/8lF7HmQ_RgY?t=2582)
- Boris Cherny（Claude Code 创造者）工作流 — [13 条技巧](tips/claude-boris-13-tips-03-jan-26.md) · [10 条技巧](tips/claude-boris-10-tips-01-feb-26.md) · [12 条技巧](tips/claude-boris-12-tips-12-feb-26.md) · [2 条技巧](tips/claude-boris-2-tips-25-mar-26.md) · [15 条技巧](tips/claude-boris-15-tips-30-mar-26.md) [![Boris](!/tags/boris-cherny.svg)](https://x.com/bcherny)

<p align="center">
  <img src="!/claude-jumping.svg" alt="section divider" width="60" height="50">
</p>

## 💡 技巧与窍门 (86)

🚫👶 = 不要过度干预

[提示](#tips-prompting) · [规划](#tips-planning) · [CLAUDE.md](#tips-claudemd) · [代理](#tips-agents) · [命令](#tips-commands) · [技能](#tips-skills) · [钩子](#tips-hooks) · [工作流](#tips-workflows) · [高级](#tips-workflows-advanced) · [Git / PR](#tips-git-pr) · [调试](#tips-debugging) · [工具](#tips-utilities) · [日常](#tips-daily)

![社区](!/tags/community.svg)

<a id="tips-prompting"></a>■ **提示 (3)**

| 技巧 | 来源 |
|-----|--------|
| 挑战 Claude — "对我这些更改进行严厉审查，直到我通过你的测试才提交 PR" 或 "向我证明这是可行的" 让 Claude 对比 main 分支和你的分支 🚫👶 | [![Boris](!/tags/boris-cherny.svg)](https://x.com/bcherny/status/2017742752566632544) |
| 在一个平庸的修复后 — "基于你现在的所有知识，废弃这个并实现优雅的解决方案" 🚫👶 | [![Boris](!/tags/boris-cherny.svg)](https://x.com/bcherny/status/2017742752566632544) |
| Claude 自己就能修复大多数 bug — 粘贴 bug，说 "修复"，不要微观管理如何做 🚫👶 | [![Boris](!/tags/boris-cherny.svg)](https://x.com/bcherny/status/2017742750473720121) |

<a id="tips-planning"></a>■ **规划/规范 (6)**

| 技巧 | 来源 |
|-----|--------|
| 始终从[计划模式](https://code.claude.com/docs/en/common-workflows)开始 | [![Boris](!/tags/boris-cherny.svg)](https://x.com/bcherny/status/2007179845336527000) |
| 从最小规范或提示开始，让 Claude 使用 [AskUserQuestion](https://code.claude.com/docs/en/cli-reference) 工具对你进行访谈，然后新建会话执行规范 | [![Thariq](!/tags/thariq.svg)](https://x.com/trq212/status/2005315275026260309) |
| 始终制定分阶段把关计划，每个阶段有多个测试（单元、自动化、集成） | |
| 启动第二个 Claude 作为资深工程师审查你的计划，或使用[跨模型](development-workflows/cross-model-workflow/cross-model-workflow.md)进行审查 | [![Boris](!/tags/boris-cherny.svg)](https://x.com/bcherny/status/2017742745365057733) |
| 在移交工作前编写详细规范并减少歧义 — 你越具体，输出就越好 | [![Boris](!/tags/boris-cherny.svg)](https://x.com/bcherny/status/2017742752566632544) |
| 原型 > PRD — 构建 20-30 个版本而不是写规范，构建成本低所以多尝试 | [![Boris](!/tags/boris-cherny.svg)](https://youtu.be/julbw1JuAz0?t=3630) [![视频](!/tags/video.svg)](https://youtu.be/julbw1JuAz0?t=3630) |

<a id="tips-claudemd"></a>■ **CLAUDE.md (7)**

| 技巧 | 来源 |
|-----|--------|
| [CLAUDE.md](https://code.claude.com/docs/en/memory) 应保持在 [200 行](https://code.claude.com/docs/en/memory#write-effective-instructions)以内。humanlayer 中 [60 行](https://www.humanlayer.dev/blog/writing-a-good-claude-md)（[仍不能 100% 保证](https://www.reddit.com/r/ClaudeCode/comments/1qn9pb9/claudemd_says_must_use_agent_claude_ignores_it_80/)）| [![Boris](!/tags/boris-cherny.svg)](https://x.com/bcherny/status/2007179840848597422) [![Dex](!/tags/dex.svg)](https://www.humanlayer.dev/blog/writing-a-good-claude-md) |
| 使用 [\<important if="..."\> 标签](https://www.hlyr.dev/blog/stop-claude-from-ignoring-your-claude-md)包裹领域特定的 CLAUDE.md 规则，防止 Claude 在文件变长时忽略它们 | [![Dex](!/tags/dex.svg)](https://www.hlyr.dev/blog/stop-claude-from-ignoring-your-claude-md) |
| 对 monorepo 使用[多个 CLAUDE.md](best-practice/claude-memory.md) — 祖先 + 后代加载 | |
| 使用 [.claude/rules/](https://code.claude.com/docs/en/memory#organize-rules-with-clauderules) 分割大型指令 | |
| [memory.md](https://code.claude.com/docs/en/memory)、constitution.md 不保证任何效果 | |
| 任何开发者都应该能启动 Claude，说"运行测试"并在首次成功 — 如果不能，你的 CLAUDE.md 缺少必要的设置/构建/测试命令 | [![Dex](!/tags/dex.svg)](https://x.com/dexhorthy/status/2034713765401551053) |
| 保持代码库整洁并完成迁移 — 部分迁移的框架会混淆模型，可能导致选择错误的模式 | [![Boris](!/tags/boris-cherny.svg)](https://youtu.be/julbw1JuAz0?t=1112) [![视频](!/tags/video.svg)](https://youtu.be/julbw1JuAz0?t=1112) |
| 使用 [settings.json](best-practice/claude-settings.md) 实现 harness 强制行为（署名、权限、模型）— 当 `attribution.commit: ""` 是确定性的时，不要在 CLAUDE.md 中写"NEVER add Co-Authored-By" | [![davila7](!/tags/davila7.svg)](https://x.com/dani_avila7/status/2036182734310195550) |

<a id="tips-agents"></a><img src="!/tags/a.svg" height="14"> **代理 (4)**

| 技巧 | 来源 |
|-----|--------|
| 创建具有[技能](https://code.claude.com/docs/en/skills)（渐进式披露）的功能特定[子代理](https://code.claude.com/docs/en/sub-agents)（额外上下文），而不是通用的 qa、backend engineer | [![Boris](!/tags/boris-cherny.svg)](https://x.com/bcherny/status/2007179850139000872) |
| 说"使用子代理"来投入更多算力解决问题 — 卸载任务保持主上下文清洁和专注 🚫👶 | [![Boris](!/tags/boris-cherny.svg)](https://x.com/bcherny/status/2017742755737555434) |
| 使用 [tmux 代理团队](https://code.claude.com/docs/en/agent-teams)和[git worktrees](https://x.com/bcherny/status/2025007393290272904)进行并行开发 | |
| 使用[测试时计算](https://code.claude.com/docs/en/sub-agents) — 分离的上下文窗口让结果更好；一个代理可能制造 bug，另一个（同模型）能发现它们 | [![Boris](!/tags/boris-cherny.svg)](https://x.com/bcherny/status/2031151689219321886) |

<a id="tips-commands"></a><img src="!/tags/c.svg" height="14"> **命令 (3)**

| 技巧 | 来源 |
|-----|--------|
| 使用[命令](https://code.claude.com/docs/en/slash-commands)而不是[子代理](https://code.claude.com/docs/en/sub-agents)来编排工作流 | [![Boris](!/tags/boris-cherny.svg)](https://x.com/bcherny/status/2007179847949500714) |
| 为每个"内循环"工作流使用[斜杠命令](https://code.claude.com/docs/en/slash-commands) — 节省重复提示，命令存放在 `.claude/commands/` 并提交到 git | [![Boris](!/tags/boris-cherny.svg)](https://x.com/bcherny/status/2007179847949500714) |
| 如果某事每天超过一次，把它变成[技能](https://code.claude.com/docs/en/skills)或[命令](https://code.claude.com/docs/en/slash-commands) — 构建 `/techdebt`、context-dump 或 analytics 命令 | [![Boris](!/tags/boris-cherny.svg)](https://x.com/bcherny/status/2017742748984742078) |

<a id="tips-skills"></a><img src="!/tags/s.svg" height="14"> **技能 (9)**

| 技巧 | 来源 |
|-----|--------|
| 使用 [context: fork](https://code.claude.com/docs/en/skills) 在隔离子代理中运行技能 — 主上下文只看到最终结果，不是中间工具调用。agent 字段让你设置子代理类型 | [![Lydia](!/tags/lydia.svg)](https://x.com/lydiahallie/status/2033603164398883042) |
| 对 monorepo 使用[子文件夹中的技能](reports/claude-skills-for-larger-mono-repos.md) | |
| 技能是文件夹，不是文件 — 使用 references/、scripts/、examples/ 子目录实现[渐进式披露](https://code.claude.com/docs/en/skills) | [![Thariq](!/tags/thariq.svg)](https://x.com/trq212/status/2033949937936085378) |
| 在每个技能中构建 Gotchas 部分 — 最高价值内容，随时间添加 Claude 的失败点 | [![Thariq](!/tags/thariq.svg)](https://x.com/trq212/status/2033949937936085378) |
| 技能描述字段是触发器，不是摘要 — 为模型写（"什么时候触发？"） | [![Thariq](!/tags/thariq.svg)](https://x.com/trq212/status/2033949937936085378) |
| 在技能中不要陈述显而易见的事 — 专注于让 Claude 脱离默认行为的因素 🚫👶 | [![Thariq](!/tags/thariq.svg)](https://x.com/trq212/status/2033949937936085378) |
| 不要在技能中 railroad Claude — 给目标和约束，不是规定性的分步指令 🚫👶 | [![Thariq](!/tags/thariq.svg)](https://x.com/trq212/status/2033949937936085378) |
| 在技能中包含脚本和库，让 Claude 组合而不是重建样板代码 | [![Thariq](!/tags/thariq.svg)](https://x.com/trq212/status/2033949937936085378) |
| 在 SKILL.md 中嵌入 `` !`command` `` 将动态 shell 输出注入提示 — Claude 在调用时运行，模型只看到结果 | [![Lydia](!/tags/lydia.svg)](https://x.com/lydiahallie/status/2034337963820327017) |

<a id="tips-hooks"></a>■ **钩子 (5)**

| 技巧 | 来源 |
|-----|--------|
| 在技能中使用[按需钩子](https://code.claude.com/docs/en/skills) — /careful 阻止破坏性命令，/freeze 阻止目录外的编辑 | [![Thariq](!/tags/thariq.svg)](https://x.com/trq212/status/2033949937936085378) |
| 使用 PreToolUse 钩子[测量技能使用](https://code.claude.com/docs/en/skills)来发现热门或触发不足的技能 | [![Thariq](!/tags/thariq.svg)](https://x.com/trq212/status/2033949937936085378) |
| 使用 [PostToolUse 钩子](https://code.claude.com/docs/en/hooks)自动格式化代码 — Claude 生成格式良好的代码，钩子处理最后 10% 避免 CI 失败 | [![Boris](!/tags/boris-cherny.svg)](https://x.com/bcherny/status/2007179852047335529) |
| 通过钩子将[权限请求](https://code.claude.com/docs/en/hooks)路由到 Opus — 让它扫描攻击并自动批准安全的请求 🚫👶 | [![Boris](!/tags/boris-cherny.svg)](https://x.com/bcherny/status/2017742755737555434) |
| 使用 [Stop 钩子](https://code.claude.com/docs/en/hooks)在回合结束时 nudging Claude 继续或验证工作 | [![Boris](!/tags/boris-cherny.svg)](https://x.com/bcherny/status/2021701059253874861) |

<a id="tips-workflows"></a>■ **工作流 (7)**

| 技巧 | 来源 |
|-----|--------|
| 避免代理愚蠢区，手动 [/compact](https://code.claude.com/docs/en/interactive-mode) 最多 50%。如果切换到新任务，使用 [/clear](https://code.claude.com/docs/en/cli-reference) 重置会话上下文 | |
| 原版 Claude Code 比任何针对更小任务的工作流都好 | |
| 使用 [/model](https://code.claude.com/docs/en/model-config) 选择模型和推理，[/context](https://code.claude.com/docs/en/interactive-mode) 查看上下文使用量，[/usage](https://code.claude.com/docs/en/costs) 检查计划限制，[/extra-usage](https://code.claude.com/docs/en/interactive-mode) 配置溢出计费，[/config](https://code.claude.com/docs/en/settings) 配置设置 — 计划模式用 Opus，编码用 Sonnet 以获得两者的最佳效果 | [![Cat](!/tags/cat-wu.svg)](https://x.com/_catwu/status/1955694117264261609) |
| 始终使用 [thinking mode](https://code.claude.com/docs/en/model-config) true（查看推理）和 [/config](https://code.claude.com/docs/en/settings) 中[输出风格](https://code.claude.com/docs/en/output-styles) Explanatory（查看带 ★ Insight 框的详细输出）以更好地理解 Claude 的决策 | [![Boris](!/tags/boris-cherny.svg)](https://x.com/bcherny/status/2007179838864666847) |
| 在提示中使用 ultrathink 关键词获得[高努力推理](https://docs.anthropic.com/en/docs/build-with-claude/extended-thinking#tips-and-best-practices) | |
| [/rename](https://code.claude.com/docs/en/cli-reference) 重要会话（如 [TODO - refactor task]）并稍后 [/resume](https://code.claude.com/docs/en/cli-reference) — 同时运行多个 Claude 时标记每个实例 | [![Cat](!/tags/cat-wu.svg)](https://every.to/podcast/how-to-use-claude-code-like-the-people-who-built-it) |
| 当 Claude 走偏时使用 [Esc Esc 或 /rewind](https://code.claude.com/docs/en/checkpointing) 回退，而不是在同一上下文中尝试修复 | |

<a id="tips-workflows-advanced"></a>■ **工作流高级 (6)**

| 技巧 | 来源 |
|-----|--------|
| 大量使用 ASCII 图表来理解你的架构 | [![Boris](!/tags/boris-cherny.svg)](https://x.com/bcherny/status/2017742759218794768) |
| 使用 [/loop](https://code.claude.com/docs/en/scheduled-tasks) 进行本地循环监控（最多 3 天）· 使用 [/schedule](https://code.claude.com/docs/en/web-scheduled-tasks) 进行云基循环任务，即使机器关闭也能运行 | |
| 使用 [Ralph Wiggum 插件](https://github.com/shanraisshan/novel-llm-26)进行长时间自主任务 | [![Boris](!/tags/boris-cherny.svg)](https://x.com/bcherny/status/2007179858435281082) |
| [/permissions](https://code.claude.com/docs/en/permissions) 使用通配符语法（Bash(npm run *)、Edit(/docs/**)）代替 dangerously-skip-permissions | [![Boris](!/tags/boris-cherny.svg)](https://x.com/bcherny/status/2007179854077407667) |
| [/sandbox](https://code.claude.com/docs/en/sandboxing) 通过文件和网络隔离减少权限提示 — 内部减少 84% | [![Boris](!/tags/boris-cherny.svg)](https://x.com/bcherny/status/2021700506465579443) [![Cat](!/tags/cat-wu.svg)](https://creatoreconomy.so/p/inside-claude-code-how-an-ai-native-actually-works-cat-wu) |
| 投资于[产品验证](https://code.claude.com/docs/en/skills)技能（signup-flow-driver、checkout-verifier）— 值得花一周时间完善 | [![Thariq](!/tags/thariq.svg)](https://x.com/trq212/status/2033949937936085378) |

<a id="tips-git-pr"></a>■ **Git / PR (5)**

| 技巧 | 来源 |
|-----|--------|
| 保持 PR 小而专注 — [中位数 118 行](tips/claude-boris-2-tips-25-mar-26.md)（一天内 141 个 PR，45K 行变更），每个 PR 一个功能，更容易审查和回退 | [![Boris](!/tags/boris-cherny.svg)](https://x.com/bcherny/status/2038552880018538749) |
| 始终[squash merge](tips/claude-boris-2-tips-25-mar-26.md) PR — 清晰的线性历史，每个功能一个提交，便于 git revert 和 git bisect | [![Boris](!/tags/boris-cherny.svg)](https://x.com/bcherny/status/2038552880018538749) |
| 经常提交 — 尝试每小时至少提交一次，任务完成后立即提交 | |
| 在同事的 PR 上标记 [@claude](https://github.com/apps/claude) 为重复审查反馈自动生成 lint 规则 — 自动化你自己退出代码审查 🚫👶 | [![Boris](!/tags/boris-cherny.svg)](https://youtu.be/julbw1JuAz0?t=2715) [![视频](!/tags/video.svg)](https://youtu.be/julbw1JuAz0?t=2715) |
| 使用 [/code-review](https://code.claude.com/docs/en/code-review) 进行多代理 PR 分析 — 在合并前捕捉 bug、安全漏洞和回归问题 | [![Boris](!/tags/boris-cherny.svg)](https://x.com/bcherny/status/2031089411820228645) |

<a id="tips-debugging"></a>■ **调试 (7)**

| 技巧 | 来源 |
|-----|--------|
| 养成截图并与 Claude 分享的习惯，遇到任何问题时 | |
| 使用 mcp（[Claude in Chrome](https://code.claude.com/docs/en/chrome)、[Playwright](https://github.com/microsoft/playwright-mcp)、[Chrome DevTools](https://developer.chrome.com/blog/chrome-devtools-mcp))让 Claude 自己查看 chrome 控制台日志 | |
| 始终让 Claude 将终端（你想看日志的）作为后台任务运行以便更好地调试 | |
| [/doctor](https://code.claude.com/docs/en/cli-reference) 诊断安装、认证和配置问题 | |
| 压缩期间的错误可以通过使用 [/model](https://code.claude.com/docs/en/model-config) 选择 1M token 模型，然后运行 [/compact](https://code.claude.com/docs/en/interactive-mode) 解决 | |
| 使用[跨模型](development-workflows/cross-model-workflow/cross-model-workflow.md)进行 QA — 如 [Codex](https://github.com/shanraisshan/codex-cli-best-practice) 进行计划和实现审查 | |
| 代理搜索（glob + grep）胜过 RAG — Claude Code 尝试并放弃了向量数据库，因为代码会 drift 失步且权限复杂 | [![Boris](!/tags/boris-cherny.svg)](https://youtu.be/julbw1JuAz0?t=3095) [![视频](!/tags/video.svg)](https://youtu.be/julbw1JuAz0?t=3095) |

<a id="tips-utilities"></a>■ **工具 (5)**

| 技巧 | 来源 |
|-----|--------|
| 使用 [iTerm](https://iterm2.com/)、[Ghostty](https://ghostty.org/)、[tmux](https://github.com/tmux/tmux) 终端而不是 IDE（[VS Code](https://code.visualstudio.com/)、[Cursor](https://www.cursor.com/)）| [![Boris](!/tags/boris-cherny.svg)](https://x.com/bcherny/status/2017742753971769626) |
| 使用 [Wispr Flow](https://wisprflow.ai) 进行语音提示（10x 效率）| |
| 使用 [claude-code-hooks](https://github.com/shanraisshan/claude-code-hooks) 获得 Claude 反馈 | |
| 使用[状态栏](https://github.com/shanraisshan/claude-code-status-line)获得上下文感知和快速压缩 | [![Boris](!/tags/boris-cherny.svg)](https://x.com/bcherny/status/2021700784019452195) |
| 探索 [settings.json](best-practice/claude-settings.md) 功能如[计划目录](best-practice/claude-settings.md#plans-directory)、[Spinner 动词](best-practice/claude-settings.md#display--ux)获得个性化体验 | [![Boris](!/tags/boris-cherny.svg)](https://x.com/bcherny/status/2021701145023197516) |

<a id="tips-daily"></a>■ **日常 (3)**

| 技巧 | 来源 |
|-----|--------|
| 每天更新 Claude Code 并以阅读[changelog](https://github.com/anthropics/claude-code/blob/main/CHANGELOG.md)开始你的一天 | |
| 关注 [r/ClaudeAI](https://www.reddit.com/r/ClaudeAI/)、[r/ClaudeCode](https://www.reddit.com/r/ClaudeCode/) | ![Reddit](https://img.shields.io/badge/-FF4500?style=flat&logo=reddit&logoColor=white) |
| 关注 [Boris](https://x.com/bcherny)、[Thariq](https://x.com/trq212)、[Cat](https://x.com/_catwu)、[Lydia](https://x.com/lydiahallie)、[Noah](https://x.com/noahzweben)、[Anthony](https://x.com/amorriscode)、[Alex](https://x.com/alexalbert__)、[Kenneth](https://x.com/neilhtennek)、[Claude](https://x.com/claudeai) | ![X](https://img.shields.io/badge/-000?style=flat&logo=x&logoColor=white) |

![Boris Cherny + 团队](!/tags/boris-team.svg)

| 文章 / 推文 | 来源 |
|-----------------|--------|
| [Claude Code 中 15 个隐藏和未被充分利用的功能 (Boris) \| 30/Mar/26](tips/claude-boris-15-tips-30-mar-26.md) | [推文](https://x.com/bcherny/status/2038454336355999749) |
| [Squash 合并 & PR 大小分布 (Boris) \| 25/Mar/26](tips/claude-boris-2-tips-25-mar-26.md) | [推文](https://x.com/bcherny/status/2038552880018538749) |
| [构建 Claude Code 的经验：我们如何使用技能 (Thariq) \| 17/Mar/26](tips/claude-thariq-tips-17-mar-26.md) | [文章](https://x.com/trq212/status/2033949937936085378) |
| [代码审查 & 测试时计算 (Boris) \| 10/Mar/26](tips/claude-boris-2-tips-10-mar-26.md) | [推文](https://x.com/bcherny/status/2031089411820228645) |
| /loop — 计划循环任务最多 3 天 (Boris) \| 07 Mar 2026 | [推文](https://x.com/bcherny/status/2030193932404150413) |
| AskUserQuestion + ASCII Markdowns (Thariq) \| 28 Feb 2026 | [推文](https://x.com/trq212/status/2027543858289250472) |
| Seeing like an Agent - 构建 Claude Code 的经验 (Thariq) \| 28 Feb 2026 | [文章](https://x.com/trq212/status/2027463795355095314) |
| Git Worktrees - Boris 使用的 5 种方式 \| 21 Feb 2026 | [推文](https://x.com/bcherny/status/2025007393290272904) |
| 构建 Claude Code 的经验：提示缓存就是一切 (Thariq) \| 20 Feb 2026 | [文章](https://x.com/trq212/status/2024574133011673516) |
| [12 种人们定制 Claude 的方式 (Boris) \| 12/Feb/26](tips/claude-boris-12-tips-12-feb-26.md) | [推文](https://x.com/bcherny/status/2021699851499798911) |
| [来自团队的 10 条 Claude Code 使用技巧 (Boris) \| 01/Feb/26](tips/claude-boris-10-tips-01-feb-26.md) | [推文](https://x.com/bcherny/status/2017742741636321619) |
| [我如何使用 Claude Code — 来自我意外简单设置的 13 条技巧 (Boris) \| 03/Jan/26](tips/claude-boris-13-tips-03-jan-26.md) | [推文](https://x.com/bcherny/status/2007179832300581177) |
| 让 Claude 使用 AskUserQuestion 工具对你进行访谈 (Thariq) \| 28/Dec/25 | [推文](https://x.com/trq212/status/2005315275026260309) |
| 始终使用计划模式，给 Claude 验证的方法，使用 /code-review (Boris) \| 27/Dec/25 | [推文](https://x.com/bcherny/status/2004711722926616680) |

![视频 / 播客](!/tags/videos-podcasts.svg)

| 视频 / 播客 | YouTube |
|-----------------|---------|
| 与 Boris Cherny 构建 Claude Code (Boris) \| 04 Mar 2026 \| The Pragmatic Engineer | [YouTube](https://youtu.be/julbw1JuAz0) |
| Claude Code 主管：编码被解决后会发生什么 (Boris) \| 19 Feb 2026 \| Lenny's Podcast | [YouTube](https://youtu.be/We7BZVKbCVw) |
| 与其创造者 Boris Cherny 深入 Claude Code (Boris) \| 17 Feb 2026 \| Y Combinator | [YouTube](https://youtu.be/PQU9o_5rHC4) |
| Boris Cherny（Claude Code 创造者）谈什么成就了他的职业生涯 (Boris) \| 15 Dec 2025 \| Ryan Peterman | [YouTube](https://youtu.be/AmdLVWMdjOk) |
| 构建者的 Claude Code 秘密 (Cat) \| 29 Oct 2025 \| Every | [YouTube](https://youtu.be/IDSAMqip6ms) |

<p align="center">
  <img src="!/claude-jumping.svg" alt="section divider" width="60" height="50">
</p>

## ☠️ 创业公司 / 商业

| Claude | 替代品 |
|-|-|
|[**代码审查 (Code Review)**](https://code.claude.com/docs/en/code-review)|[Greptile](https://greptile.com), [CodeRabbit](https://coderabbit.ai), [Devin Review](https://devin.ai), [OpenDiff](https://opendiff.com), [Cursor BugBot](https://bugbot.dev)|
|[**语音听写 (Voice Dictation)**](https://code.claude.com/docs/en/voice-dictation)|[Wispr Flow](https://wisprflow.ai), [SuperWhisper](https://superwhisper.com/)|
|[**远程控制 (Remote Control)**](https://code.claude.com/docs/en/remote-control)|[OpenClaw](https://openclaw.ai/)
|[**Cowork**](https://claude.com/blog/cowork-research-preview)|[OpenAI Operator](https://openai.com/operator), [AgentShadow](https://agentshadow.ai)
|[**任务 (Tasks)**](https://x.com/trq212/status/2014480496013803643)|[Beads](https://github.com/steveyegge/beads)
|[**计划模式 (Plan Mode)**](https://code.claude.com/docs/en/common-workflows)|[Agent OS](https://github.com/buildermethods/agent-os)|
|[**技能 / 插件 (Skills / Plugins)**](https://code.claude.com/docs/en/plugins)|YC AI wrapper 创业公司 ([reddit](https://reddit.com/r/ClaudeAI/comments/1r6bh4d/claude_code_skills_are_basically_yc_ai_startup/))|

<p align="center">
  <img src="!/claude-jumping.svg" alt="section divider" width="60" height="50">
</p>

<a id="billion-dollar-questions"></a>
![十亿美元问题](!/tags/billion-dollar-questions.svg)

*如果你有答案，请发邮件至 shanraisshan@gmail.com*

**记忆与指令 (4)**

1. 你到底应该把什么放进 CLAUDE.md — 又应该把什么排除？
2. 如果你已经有 CLAUDE.md，是否真的需要单独的 constitution.md 或 rules.md？
3. 你应该多久更新 CLAUDE.md，又如何知道它何时变得过时？
4. 为什么 Claude 仍然忽略 CLAUDE.md 指令 — 即使它们用大写字母写着 MUST？([reddit](https://reddit.com/r/ClaudeCode/comments/1qn9pb9/claudemd_says_must_use_agent_claude_ignores_it_80/))

**代理、技能与工作流 (6)**

1. 什么时候应该使用命令 vs 代理 vs 技能 — 什么时候原版 Claude Code 就更好？
2. 随着模型改进，你应该多久更新你的代理、命令和工作流？
3. 给子代理详细的 persona 是否提高质量？研究/QA 子代理的"完美 persona/prompt"是什么样的？
4. 你应该依赖 Claude Code 的内置计划模式 — 还是构建自己的计划命令/代理来强制执行团队工作流？
5. 如果你有个人技能（如 /implement 带你的编码风格），你如何整合社区技能（如 /simplify）而不冲突 — 当它们不一致时谁优先？
6. 我们到了吗？我们能将现有代码库转换成规范，删除代码，并让 AI 仅从这些规范重新生成完全相同的代码吗？

**规范与文档 (3)**

1. 你的仓库中的每个功能都应该有一个规范作为 markdown 文件吗？
2. 你需要多久更新规范以便在新功能实现时不变得过时？
3. 在实现新功能时，你如何处理对其他功能规范的涟漪效应？

<p align="center">
  <img src="!/claude-jumping.svg" alt="section divider" width="60" height="50">
</p>

## 报告

<p align="center">
  <a href="reports/claude-agent-sdk-vs-cli-system-prompts.md"><img src="https://img.shields.io/badge/Agent_SDK_vs_CLI-555?style=for-the-badge" alt="Agent SDK vs CLI"></a>
  <a href="reports/claude-in-chrome-v-chrome-devtools-mcp.md"><img src="https://img.shields.io/badge/Browser_Automation_MCP-555?style=for-the-badge" alt="浏览器自动化 MCP"></a>
  <a href="reports/claude-global-vs-project-settings.md"><img src="https://img.shields.io/badge/Global_vs_Project_Settings-555?style=for-the-badge" alt="全局 vs 项目设置"></a>
  <a href="reports/claude-skills-for-larger-mono-repos.md"><img src="https://img.shields.io/badge/Skills_in_Monorepos-555?style=for-the-badge" alt="Monorepos 中的技能"></a>
  <br>
  <a href="reports/claude-agent-memory.md"><img src="https://img.shields.io/badge/Agent_Memory-555?style=for-the-badge" alt="代理记忆"></a>
  <a href="reports/claude-advanced-tool-use.md"><img src="https://img.shields.io/badge/Advanced_Tool_Use-555?style=for-the-badge" alt="高级工具使用"></a>
  <a href="reports/claude-usage-and-rate-limits.md"><img src="https://img.shields.io/badge/Usage_&_Rate_Limits-555?style=for-the-badge" alt="使用量 & 速率限制"></a>
  <a href="reports/claude-agent-command-skill.md"><img src="https://img.shields.io/badge/Agents_vs_Commands_vs_Skills-555?style=for-the-badge" alt="代理 vs 命令 vs 技能"></a>
  <br>
  <a href="reports/llm-day-to-day-degradation.md"><img src="https://img.shields.io/badge/LLM_Degradation-555?style=for-the-badge" alt="LLM 退化"></a>
</p>

<p align="center">
  <img src="!/claude-jumping.svg" alt="section divider" width="60" height="50">
</p>

![如何使用](!/tags/how-to-use.svg)

```
1. 像课程一样阅读这个仓库，在尝试使用之前先了解命令、代理、技能和钩子是什么。
2. 克隆这个仓库并玩示例，尝试 /weather-orchestrator，听钩子声音，运行代理团队，这样你可以看到事情实际上是如何工作的。
3. 去你自己的项目并让 Claude 建议你应该从这个仓库添加什么最佳实践，给它这个仓库作为参考，这样它知道什么是可能的。
```

<p align="center">
  <img src="!/claude-jumping.svg" alt="section divider" width="60" height="50">
</p>

<p align="center">
  <a href="https://github.com/trending?since=monthly"><img src="!/root/github-trending.png" alt="GitHub Trending" width="1200"></a><br>
  ✨2026 年 3 月在 Github 上热门✨
</p>

## 其他仓库

<a href="https://github.com/shanraisshan/claude-code-hooks"><img src="!/claude-speaking.svg" alt="Claude Code Hooks" width="40" height="40" align="center"></a> <a href="https://github.com/shanraisshan/claude-code-hooks"><strong>claude-code-hooks</strong></a> · <a href="https://github.com/shanraisshan/codex-cli-best-practice"><img src="!/codex-jumping.svg" alt="Codex CLI" width="40" height="40" align="center"></a> <a href="https://github.com/shanraisshan/codex-cli-best-practice"><strong>codex-cli-best-practice</strong></a> · <a href="https://github.com/shanraisshan/codex-cli-hooks"><img src="!/codex-speaking.svg" alt="Codex CLI Hooks" width="40" height="40" align="center"></a> <a href="https://github.com/shanraisshan/codex-cli-hooks"><strong>codex-cli-hooks</strong></a>

## 开发者

![开发者](!/tags/developed-by.svg)

> | 工作流 | 描述 |
> |----------|-------------|
> | /workflows:development-workflows | 通过并行研究所有 8 个工作流仓库更新开发工作流表和跨工作流分析报告 |
> | /workflows:best-practice:workflow-concepts | 用最新 Claude Code 功能和概念更新 README 概念部分 |
> | /workflows:best-practice:workflow-claude-settings | 追踪 Claude Code 设置报告变更并找出需要更新的内容 |
> | /workflows:best-practice:workflow-claude-subagents | 追踪 Claude Code 子代理报告变更并找出需要更新的内容 |
> | /workflows:best-practice:workflow-claude-commands | 追踪 Claude Code 命令报告变更并找出需要更新的内容 |
> | /workflows:best-practice:workflow-claude-skills | 追踪 Claude Code 技能报告变更并找出需要更新的内容 |

[![Claude for OSS](!/tags/claude-for-oss.svg)](https://claude.com/contact-sales/claude-for-oss)
[![Claude Community Ambassador](!/tags/claude-community-ambassador.svg)](https://claude.com/community/ambassadors)
[![Claude Certified Architect](!/tags/claude-certified-architect.svg)](https://anthropic.skilljar.com/claude-certified-architect-foundations-access-request)
[![Anthropic Academy](!/tags/anthropic-academy.svg)](https://anthropic.skilljar.com/)

## Star 历史

[![Star History Chart](https://api.star-history.com/svg?repos=shanraisshan/claude-code-best-practice&type=Date)](https://star-history.com/#shanraisshan/claude-code-best-practice&Date)