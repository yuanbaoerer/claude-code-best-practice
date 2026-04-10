# Claude Code Best Practice - 报告汇总

本文档汇总了所有研究报告，涵盖配置系统、使用限制、工具使用、MCP集成等核心主题。

---

## 图例

| 标记 | 含义 | 说明 |
|------|------|------|
| 🤖 **自动运行** | Claude Code 自动执行 | 无需用户触发，Claude根据上下文自动运行 |
| 👆 **用户指挥** | 需要用户明确触发 | 用户需要输入命令或明确授权才能执行 |

---

## 目录

1. [Global vs Project-Level Settings](#1-global-vs-project-level-settings)
2. [Usage, Rate Limits & Extra Usage](#2-usage-rate-limits--extra-usage)
3. [LLM Day-to-Day Degradation](#3-llm-day-to-day-degradation)
4. [Browser Automation MCP Comparison](#4-browser-automation-mcp-comparison)
5. [Advanced Tool Use Patterns](#5-advanced-tool-use-patterns)
6. [Agent SDK vs CLI System Prompts](#6-agent-sdk-vs-cli-system-prompts)
7. [Skills Discovery in Monorepos](#7-skills-discovery-in-monorepos)
8. [Agent vs Command vs Skill](#8-agent-vs-command-vs-skill)
9. [Agent Memory](#9-agent-memory)

---

## 1. Global vs Project-Level Settings

**来源**: [reports/claude-global-vs-project-settings.md](reports/claude-global-vs-project-settings.md)

### 核心概念

Claude Code 使用**作用域层级**架构，部分功能仅存在于全局（`~/.claude/`），部分功能在全局和项目级别（`.claude/`）同时存在。

### Global-Only 功能（仅在 `~/.claude/`）

| 功能 | 位置 | 用途 | 运行方式 |
|------|------|------|---------|
| **Tasks** 🤖 | `~/.claude/tasks/` | 跨会话和代理的持久任务列表 | 🤖 自动运行 — 通过 `CLAUDE_CODE_TASK_LIST_ID` 跨会话同步 |
| **Agent Teams** 🤖 | `~/.claude/teams/` | 多代理协调配置（实验性） | 🤖 自动运行 — 配置后自动协调多代理 |
| **Auto Memory** 🤖 | `~/.claude/projects/<hash>/memory/` | 每个项目的自动学习（个人，永不共享） | 🤖 自动运行 — Claude自动写入学习内容 |
| **Credentials & OAuth** 👆 | System keychain + `~/.claude.json` | API密钥、OAuth令牌 | 👆 用户配置 — 需手动设置 |
| **Keybindings** 👆 | `~/.claude/keybindings.json` | 自定义键盘快捷键 | 👆 用户配置 — 需手动定义 |
| **MCP User Servers** 👆 | `~/.claude.json` (`mcpServers` key) | 跨项目的个人MCP服务器 | 👆 用户配置 — 需在配置文件中声明 |

### Dual-Scope 功能（项目级别优先）

| 功能 | 全局 (`~/.claude/`) | 项目 (`.claude/`) | 优先级 | 运行方式 |
|------|---------------------|-------------------|--------|---------|
| **CLAUDE.md** | `~/.claude/CLAUDE.md` | `./CLAUDE.md` or `.claude/CLAUDE.md` | 项目覆盖全局 | 🤖 自动运行 — 文件内容自动加载到上下文 |
| **Settings** | `~/.claude/settings.json` | `.claude/settings.json` + `.claude/settings.local.json` | 项目 > 全局 | 🤖 自动运行 — 设置自动生效 |
| **Rules** | `~/.claude/rules/*.md` | `.claude/rules/*.md` | 项目覆盖 | 🤖 自动运行 — 规则自动注入上下文 |
| **Agents/Subagents** | `~/.claude/agents/*.md` | `.claude/agents/*.md` | 项目覆盖 | 👆 用户指挥 — 通过 Agent 工具调用 |
| **Commands** | `~/.claude/commands/*.md` | `.claude/commands/*.md` | 两者可用 | 👆 用户指挥 — 通过 `/command-name` 调用 |
| **Skills** | `~/.claude/skills/` | `.claude/skills/` | 两者可用 | 🤖 自动运行 — 通过 description 自动匹配调用 |
| **Hooks** | `~/.claude/hooks/` | `.claude/hooks/` | 两者执行 | 🤖 自动运行 — 事件触发自动执行 |
| **MCP Servers** | `~/.claude.json` (user scope) | `.mcp.json` (project scope) | 本地 > 项目 > 用户 | 🤖 自动运行 — 工具自动可用 |

### 设置优先级（从高到低）

| 优先级 | 位置 | 作用域 | 版本控制 | 用途 |
|--------|------|--------|---------|------|
| 1 | Command line flags | Session | N/A | 单会话覆盖 |
| 2 | `.claude/settings.local.json` | Project | No (git-ignored) | 个人项目特定 |
| 3 | `.claude/settings.json` | Project | Yes (committed) | 团队共享设置 |
| 4 | `~/.claude/settings.local.json` | User | N/A | 个人全局覆盖 |
| 5 | `~/.claude/settings.json` | User | N/A | 全局个人设置 |

### Tasks 系统

v2.1.16引入的新任务系统，替代废弃的TodoWrite：

| 特性 | 旧Todos | 新Tasks | 运行方式 |
|------|---------|---------|---------|
| 作用域 | 单会话 | 跨会话、跨代理 | 🤖 自动运行 |
| 依赖关系 | 无 | 完整依赖图 | 🤖 自动运行 |
| 存储 | 仅内存 | 文件系统 (`~/.claude/tasks/`) | 🤖 自动运行 |
| 持久性 | 会话结束丢失 | 跨重启和崩溃存活 | 🤖 自动运行 |
| 多会话 | 不可能 | 通过 `CLAUDE_CODE_TASK_LIST_ID` | 👆 用户指挥 — 需设置环境变量 |

### 设计原则

| 类别 | 作用域 | 理由 | 运行方式 |
|------|--------|------|---------|
| **协调状态**（tasks, teams） | Global-only | 需要超越任何单一项目持久化 | 🤖 自动运行 |
| **安全状态**（credentials, OAuth） | Global-only | 防止意外提交到版本控制 | 👆 用户配置 |
| **个人学习**（auto-memory） | Global-only | 用户特定，非团队共享 | 🤖 自动运行 |
| **输入偏好**（keybindings） | Global-only | 用户肌肉记忆，非项目特定 | 👆 用户配置 |
| **配置**（settings, rules, agents） | 两者都有 | 团队需要共享项目特定行为 | 🤖 自动运行 |
| **工作流定义**（commands, skills） | 两者都有 | 可以是个人或团队共享 | 👆 命令=用户指挥 / 🤖 skill=自动运行 |

---

## 2. Usage, Rate Limits & Extra Usage

**来源**: [reports/claude-usage-and-rate-limits.md](reports/claude-usage-and-rate-limits.md)

### 内置命令

| 命令 | 描述 | 适用用户 | 运行方式 |
|------|------|---------|---------|
| `/usage` | 检查计划限制和速率限制状态 | Pro, Max 5x, Max 20x | 👆 用户指挥 |
| `/extra-usage` | 配置超额付费，当限制达到时继续工作 | Pro, Max 5x, Max 20x | 👆 用户指挥 |
| `/cost` | 显示当前会话的令牌使用和费用 | API key用户 | 👆 用户指挥 |

### `/extra-usage` 工作原理

1. 达到计划速率限制（限制每5小时重置）
2. 如果启用了extra usage且有可用资金，Claude Code无缝继续
3. 溢出令牌按**标准API费率**计费，与订阅费分开

### 关键细节

| 详情 | 值 |
|------|-----|
| 每日兑换限制 | $2,000/天 |
| 计费 | 与订阅分开，按标准API费率 |
| 限制重置窗口 | 每5小时 |

### Fast Mode 和 Extra Usage

- Fast mode (`/fast`) 使用 Claude Opus 4.6
- Fast mode使用**始终从第一个令牌计入extra usage**
- 即使订阅计划还有剩余，也不例外
- 使用 `/fast` 需要启用extra usage并有资金

### CLI 启动标志（API用户）

| 标志 | 描述 |
|------|------|
| `--max-budget-usd <AMOUNT>` | 停止前的最大美元金额 |
| `--max-turns <NUMBER>` | 限制agent轮次数 |

---

## 3. LLM Day-to-Day Degradation

**来源**: [reports/llm-day-to-day-degradation.md](reports/llm-day-to-day-degradation.md)

### 核心发现

| 问题 | 答案 |
|------|------|
| 模型权重在发布后会改变吗？ | **否** — 所有提供商确认 |
| 模型行为日复一日会不同吗？ | **是** — 已证明±8-14%方差 |
| 是故意的"削弱"吗？ | **否** — 没有故意降级的证据 |
| 基础设施bug是真实的吗？ | **是** — Anthropic确认了影响高达16%请求的3个bug |
| 部分是心理因素吗？ | **是** — 确认偏差和蜜月效应是真实的 |
| 系统提示/训练后更改会影响吗？ | **是** — 在提供商中有记录 |
| 用户应该相信自己的感知吗？ | **部分** — 真实原因存在，但感知被放大 |

### 完整推理栈

模型权重是冻结的，但**其之上的九个层**可以独立影响体验：

```
┌──────────────────────────────────────────────┐
│  YOUR SESSION CONTEXT                        │  ← 会话内降级
│  (accumulated errors, long conversations)     │
├──────────────────────────────────────────────┤
│  SYSTEM PROMPT                               │  ← 定期更新
│  (safety rules, behavior instructions)       │
├──────────────────────────────────────────────┤
│  POST-TRAINING (RLHF / Fine-tuning)         │  ← 可以悄悄更新
│  (instruction following, safety alignment)   │
├──────────────────────────────────────────────┤
│  SAMPLING PARAMETERS                         │  ← 可以在服务端调优
│  (temperature, top-p, top-k)                 │
├──────────────────────────────────────────────┤
│  SPECULATIVE DECODING                        │  ← 草稿模型质量变化
│  (draft model predictions + verification)    │
├──────────────────────────────────────────────┤
│  MoE ROUTING / BATCH COMPOSITION             │  ← 已证明±8-14%方差
│  (which experts activate per request)        │
├──────────────────────────────────────────────┤
│  HARDWARE ROUTING                            │  ← TPU vs GPU vs Trainium
│  (which cluster serves your request)         │
├──────────────────────────────────────────────┤
│  QUANTIZATION LEVEL                          │  ← 负载下可能变化
│  (FP16 vs INT8 vs INT4 precision)           │
├──────────────────────────────────────────────┤
│  COMPILER & RUNTIME                         │  ← XLA bug已证实
│  (XLA:TPU, CUDA, hardware-specific code)    │
├──────────────────────────────────────────────┤
│  MODEL WEIGHTS (FROZEN)                      │  ← 这些不变
│  (billions of learned parameters)             │
└──────────────────────────────────────────────┘
```

### 已证实的根本原因

#### 1. Anthropic的2025年9月事后分析

Anthropic公布了三个独立的基础设施bug：
- **Bug #1**: 上下文窗口路由错误（影响16%的请求）
- **Bug #2**: TPU输出损坏（产生乱码字符）
- **Bug #3**: XLA:TPU编译器错误编译（最严重）

#### 2. MoE路由方差

Scale AI的研究揭示了关键发现：
> "稀疏MoE和批处理推理的组合创造了不可预测的结果，因为批次的组成可以决定你的查询被路由到哪个专家。"

| 提供商 | 日间分数方差 |
|--------|------------|
| OpenAI (GPT-4 variants) | ±10–12% |
| Anthropic (Claude variants) | ±8–11% |
| Google (Gemini variants) | ±9–14% |

#### 3. 系统提示和训练后更新

- 系统提示可以随时更新
- 公司可以在不更改基础模型权重的情况下更新**微调和RLHF**

### 贡献因素

| 因素 | 描述 | 运行方式 |
|------|------|---------|
| **上下文窗口污染** | 长编码会话中，早期错误累积在上下文中 | 🤖 自动运行 — 会话内自动累积 |
| **量化负载** | 提供商可能在负载下使用量化版本（FP16 → INT8/INT4） | 🤖 自动运行 — 提供商自动调整 |
| **投机解码** | 草稿模型质量因领域和上下文而异 | 🤖 自动运行 — 服务端自动处理 |
| **确认偏差** | 一旦有人发推"Claude今天很笨"，你开始注意到每一个错误 | 👆 用户感知 — 用户主观感受 |
| **LLM随机性** | 相同的提示每次都可能产生不同的输出 | 🤖 自动运行 — 模型固有特性 |

### 实用建议

- 使用 `/compact` 或开始新的会话当质量感觉不对时
- 这是你可以做的**最可操作的**事情

---

## 4. Browser Automation MCP Comparison

**来源**: [reports/claude-in-chrome-v-chrome-devtools-mcp.md](reports/claude-in-chrome-v-chrome-devtools-mcp.md)

### 三种工具对比

| 工具 | 主要用途 | Token效率 | 跨浏览器 | CI/CD |
|------|----------|----------|---------|-------|
| **Chrome DevTools MCP** | 调试和性能 | 19.0k (9.5%) | ❌ | ✅ |
| **Claude in Chrome** | 通用浏览器自动化 | 15.4k (7.7%) | ❌ | ❌ |
| **Playwright MCP** | UI测试和E2E | 13.7k (6.8%) | ✅ | ✅ |

### 工具详情

#### Chrome DevTools MCP (26工具)

```
输入自动化(8):    click, drag, fill, fill_form, handle_dialog,
                  hover, press_key, upload_file
导航(6):          close_page, list_pages, navigate_page,
                  new_page, select_page, wait_for
性能(3):          performance_analyze_insight,
                  performance_start_trace, performance_stop_trace
网络(2):          get_network_request, list_network_requests
调试(5):          evaluate_script, get_console_message,
                  list_console_messages, take_screenshot,
                  take_snapshot
```

#### Claude in Chrome (16工具)

```
浏览器控制:        navigate, read_page, find, computer
表单交互:          form_input, javascript_tool
媒体:              upload_image, get_page_text, gif_creator
标签管理:          tabs_context_mcp, tabs_create_mcp
开发:              read_console_messages, read_network_requests
工具:              shortcuts_list, shortcuts_execute,
                  resize_window, update_plan
```

#### Playwright MCP (21工具)

```
导航:              navigate, goBack, goForward, reload
交互:              click, fill, select, hover, press,
                  drag, uploadFile
元素查询:          getElement, getElements, waitForSelector
断言:              assertVisible, assertText, assertTitle
页面状态:          screenshot, getAccessibilityTree,
                  evaluateScript
浏览器管理:        newPage, closePage
```

### 安全考虑

| 工具 | 安全评级 | 关键问题 |
|------|---------|---------|
| **Chrome DevTools MCP** | ✅ 良好 | 隔离浏览器profile，默认安全 |
| **Claude in Chrome** | ⚠️ 风险 | 无缓解措施时23.6%攻击成功率 |
| **Playwright MCP** | ✅ 最佳 | Microsoft支持，成熟安全模型 |

### 推荐设置

```bash
# 安装Playwright和Chrome DevTools MCP
npx playwright install
claude mcp add playwright -s user -- npx @playwright/mcp@latest
claude mcp add chrome-devtools -s user -- npx chrome-devtools-mcp@latest
```

### 推荐工作流

```
1. 开发       → Claude Code (终端)
2. 测试       → Playwright MCP (E2E, 跨浏览器)
3. 调试       → Chrome DevTools MCP (性能, 网络)
4. 验证       → Claude in Chrome (快速视觉检查)
5. CI/CD      → Playwright MCP (无头, 自动化)
```

---

## 5. Advanced Tool Use Patterns

**来源**: [reports/claude-advanced-tool-use.md](reports/claude-advanced-tool-use.md)

### 功能概览

| 功能 | 解决的问题 | Token节省 | 可用性 |
|------|----------|----------|--------|
| Programmatic Tool Calling | 多步agent循环消耗大量token | ~37%减少 | API, Foundry (GA) |
| Dynamic Filtering | Web搜索/获取结果使上下文膨胀 | ~24%更少输入token | API, Foundry (GA) |
| Tool Search Tool | 太多工具定义使上下文膨胀 | ~85%减少 | API, Foundry (GA) |
| Tool Use Examples | 模式无法仅用schema表达 | 72% → 90%准确率 | API, Foundry (GA) |

### Programmatic Tool Calling (PTC)

#### 范式转变

**传统方式（每个工具调用需要完整模型往返）**：
```
用户提示 → Claude → 工具调用1 → 响应1 → Claude → 工具调用2 → 响应2 → Claude → 工具调用3 → 响应3 → Claude → 最终答案
```

**PTC方式（Claude编写Python代码编排所有工具）**：
```
用户提示 → Claude → 编写Python脚本 → 脚本内部调用工具1、工具2、工具3 → stdout → Claude → 最终答案
```

#### 关键配置

```json
{
  "tools": [
    {
      "type": "code_execution_20250825",
      "name": "code_execution"
    },
    {
      "name": "query_database",
      "allowed_callers": ["code_execution_20250825"]
    }
  ]
}
```

#### 高级模式

**批处理** — 1次推理pass处理N个项目：
```python
regions = ["West", "East", "Central", "North", "South"]
results = {}
for region in regions:
    data = await query_database(f"SELECT SUM(revenue) FROM sales WHERE region='{region}'")
    results[region] = data[0]["revenue"]
```

**早期终止** — 满足成功条件时停止：
```python
for endpoint in endpoints:
    status = await check_health(endpoint)
    if status == "healthy":
        print(f"Found healthy endpoint: {endpoint}")
        break
```

### Dynamic Filtering

Web搜索和获取工具将完整HTML页面倾倒到Claude的上下文窗口。大多数内容是无关的——导航、广告、样板文件。

**之前**：
```
查询 → 搜索结果 → 获取完整HTML × N页 → 所有内容进入上下文 → Claude处理所有内容
```

**之后**：
```
查询 → 搜索结果 → Claude编写过滤代码 → 代码仅提取相关内容 → 过滤结果进入上下文
```

### Claude Code相关性

| 功能 | Claude Code状态 | 操作 |
|------|----------------|------|
| Tool Search | 自v2.1.7内置为MCPSearch自动模式 | 如果MCP工具多，调整`ENABLE_TOOL_SEARCH=auto:N` |
| Dynamic Filtering | CLI不可用（API级web工具） | 与执行web研究的Agent SDK用户相关 |
| PTC | CLI不可用 | 与构建自定义代理的Agent SDK用户相关 |
| Tool Use Examples | CLI不可配置 | 与自定义MCP服务器作者相关 |

---

## 6. Agent SDK vs CLI System Prompts

**来源**: [reports/claude-agent-sdk-vs-cli-system-prompts.md](reports/claude-agent-sdk-vs-cli-system-prompts.md)

### 系统提示对比

#### Claude CLI (Claude Code)

使用**模块化系统提示架构**，约269个token的基础提示：

| 组件 | 描述 | 加载方式 |
|------|------|---------|
| **基础系统提示** | 核心指令和行为 | 始终（约269 tokens） |
| **工具指令** | 18+内置工具 | 始终 |
| **编码指南** | 代码风格、格式规则、安全实践 | 始终 |
| **安全规则** | 拒绝规则、注入防御、伤害预防 | 始终 |
| **响应样式** | 语气、详细程度、解释深度 | 始终 |
| **环境上下文** | 工作目录、git状态、平台信息 | 始终 |
| **项目上下文** | CLAUDE.md内容、设置、hooks配置 | 条件 |
| **子代理提示** | Plan模式、Explore代理、Task代理 | 条件 |

#### Claude Agent SDK

默认使用**最小系统提示**：

| 组件 | 描述 | Token影响 |
|------|------|---------|
| **基本工具指令** | 仅明确提供的工具 | 最小 |
| **基本安全** | 最小安全指令 | 最小 |

### 关键发现

**Claude Messages API不提供用于可重现性的种子参数。** 这是根本性的架构限制。

### 输出一致性保证

**无保证。** 即使使用匹配的系统提示、相同输入和`temperature=0`，也无法保证输出相同，因为：
- 基础设施级别的非确定性
- MoE路由变化
- 批处理/调度差异

### 最大一致性配置

```typescript
// Agent SDK配置
const response = await client.messages.create({
  model: "claude-sonnet-4-20250514",
  max_tokens: 1024,
  system: "Your exact system prompt matching CLI",
  messages: [{ role: "user", content: "What is the capital of Norway?" }],
  temperature: 0
});
```

### 实用建议

1. **不要依赖位完美的可重现性**
2. **构建能容忍轻微输出变化的应用**
3. **使用结构化输出和验证**
4. **对于一致性关键的生产管道**：使用缓存、结构化输出验证、确定性逻辑

---

## 7. Skills Discovery in Monorepos

**来源**: [reports/claude-skills-for-larger-mono-repos.md](reports/claude-skills-for-larger-mono-repos.md)

### 重要区别

**Skills与CLAUDE.md的加载行为不同。** 虽然CLAUDE.md文件向上遍历目录树（祖先加载），但skills使用不同的发现机制。

### 标准技能位置

| 位置 | 路径 | 适用于 |
|------|------|-------|
| Enterprise | Managed settings | 组织中的所有用户 |
| Personal | `~/.claude/skills/<skill-name>/SKILL.md` | 所有项目 |
| Project | `.claude/skills/<skill-name>/SKILL.md` | 仅此项目 |
| Plugin | `<plugin>/skills/<skill-name>/SKILL.md` | 插件启用位置 |

### 自动发现

在子目录中处理文件时，Claude Code自动从嵌套的`.claude/skills/`目录发现skills。

### Monorepo结构示例

```
/mymonorepo/
├── .claude/
│   └── skills/
│       └── shared-conventions/SKILL.md    # 项目级skill
├── packages/
│   ├── frontend/
│   │   ├── .claude/
│   │   │   └── skills/
│   │   │       └── react-patterns/SKILL.md  # 前端特定skill
│   ├── backend/
│   │   ├── .claude/
│   │   │   └── skills/
│   │   │       └── api-design/SKILL.md      # 后端特定skill
│   └── shared/
│       ├── .claude/
│       │   └── skills/
│       │       └── utils-patterns/SKILL.md  # 共享工具skill
```

### 关键行为：Description vs Full Content

- **Descriptions**: 始终在上下文中（在字符预算内）
- **Full content**: 调用时加载（按需）

> 注意：带有预加载skills的子代理工作方式不同——完整skill内容在启动时注入。

### 优先级（同名时）

| 优先级 | 位置 | 作用域 |
|--------|------|--------|
| 1 (最高) | Enterprise | 组织范围 |
| 2 | Personal (`~/.claude/skills/`) | 所有项目 |
| 3 (最低) | Project (`.claude/skills/`) | 仅此项目 |

### 最佳实践

1. **将共享工作流放在根`.claude/skills/`** — 仓库范围约定、提交工作流
2. **将包特定skills放在包`.claude/skills/`** — 框架特定模式、组件约定
3. **对危险skills使用`disable-model-invocation: true`** — 部署或破坏性skills应需要明确用户调用
4. **保持skill描述简洁** — 描述始终在上下文中，冗长描述浪费空间
5. **在skill名称中使用命名空间** — 考虑前缀包名（如`frontend-review`、`backend-deploy`）

---

## 8. Agent vs Command vs Skill

**来源**: [reports/claude-agent-command-skill.md](reports/claude-agent-command-skill.md)

### 一览

| | Agent | Command | Skill | 运行方式 |
|---|---|---|---|---------|
| **位置** | `.claude/agents/<name>.md` | `.claude/commands/<name>.md` | `.claude/skills/<name>/SKILL.md` | — |
| **上下文** | 独立子代理进程 | 内联（主对话） | 内联（主对话） | — |
| **用户可调用** | 否 — 通过Claude或Agent工具调用 | 是 — `/command-name` | 是 — `/skill-name` | 👆 用户指挥 |
| **自动调用** | 是 — 通过`description`字段 | 否 | 是 — 通过`description`字段 | 🤖 自动运行 |
| **接受参数** | 通过`prompt`参数 | `$ARGUMENTS`, `$0`, `$1` | `$ARGUMENTS`, `$0`, `$1` | — |
| **动态上下文注入** | 否 | 是 — `` !`command` `` | 是 — `` !`command` `` | — |
| **独立上下文窗口** | 是 — 隔离 | 否 — 共享主会话 | 否 — 共享主会话（除非`context: fork`） | — |

### 使用场景

#### 使用Agent当：
- 任务是**自主和多步的** — 代理需要探索、决定和行动
- 需要**上下文隔离** — 工作不应污染主对话窗口
- 代理需要**持久记忆**跨会话（如学习模式的代码审查器）
- 需要**预加载领域知识**通过skills而不弄乱主上下文
- 任务适合在**后台**运行或在**git worktree**中运行
- 需要**工具限制**或不同的**权限模式**

#### 使用Command当：
- 需要**用户-initiated入口点** — 用户明确触发的工作流
- 工作流涉及**编排**其他代理或skills
- 想要**保持上下文精益** — 命令内容在用户触发前不注入

#### 使用Skill当：
- 想要Claude基于用户意图**自动调用** — skill描述被注入用于语义匹配
- 任务是**可重用过程**可以从多地方调用（命令、代理或Claude本身）
- 需要**代理预加载** — 在启动时将领域知识烘焙到特定代理

### Command → Agent → Skill 架构

| 步骤 | 组件 | 运行方式 | 说明 |
|------|------|---------|------|
| 1 | 用户触发 `/command` | 👆 用户指挥 | 用户明确输入命令 |
| 2 | Command编排工作流 | 👆 用户指挥 | Command内容在用户触发后才注入 |
| 3 | Command调用Agent | 🤖 自动运行 | 通过Agent工具自动调用 |
| 4 | Agent使用预加载Skill | 🤖 自动运行 | 领域知识在启动时注入 |
| 5 | Command调用Skill | 🤖 自动运行 | 通过Skill工具自动调用 |

### 解析顺序

当多个机制匹配相同意图时，Claude偏好**最轻量级的选项**：

| 优先级 | 机制 | 运行方式 |
|--------|------|---------|
| 1 | Skill（内联，无上下文开销） | 🤖 自动运行 — 自动匹配调用 |
| 2 | Agent（独立上下文，自主） | 🤖 自动运行 — 自动匹配（如果skill不可用或任务复杂） |
| 3 | Command（从不 — 需要明确的/） | 👆 用户指挥 — 仅当用户输入 `/time-command` 时 |

### 工作示例："What is the current time?"

| 机制 | 会触发吗？ | 原因 |
|------|----------|------|
| `time-command` | 否 | 命令**永远不会自动调用**。用户需要明确输入`/time-command`。 |
| `time-agent` | **可能** | 代理的描述说*"Use this agent to display the current time..."*，Claude可能通过Agent工具生成它。但代理在**单独的上下文窗口**中运行，对于这个简单任务来说更重。 |
| `time-skill` | **最可能** | Skill的描述说*"Display the current time in Pakistan Standard Time..."*，Claude匹配并通过Skill工具调用它。由于它**内联运行**且没有上下文开销，是最有效的匹配。 |

---

## 9. Agent Memory

**来源**: [reports/claude-agent-memory.md](reports/claude-agent-memory.md)

### 概述

在**Claude Code v2.1.33**（2026年2月）引入，`memory` frontmatter字段为每个子代理提供自己的持久markdown知识库。

```yaml
---
name: code-reviewer
description: Reviews code for quality and best practices
tools: Read, Write, Edit, Bash
model: sonnet
memory: user
---

You are a code reviewer. As you review code, update your agent memory with
patterns, conventions, and recurring issues you discover.
```

### 内存作用域

| 作用域 | 存储位置 | 版本控制 | 共享 | 适用于 | 运行方式 |
|--------|----------|---------|------|---------|---------|
| `user` | `~/.claude/agent-memory/<agent-name>/` | 否 | 否 | 跨项目知识（推荐默认） | 🤖 自动运行 — 代理自动读写 |
| `project` | `.claude/agent-memory/<agent-name>/` | 是 | 是 | 团队应共享的项目特定知识 | 🤖 自动运行 — 代理自动读写 |
| `local` | `.claude/agent-memory-local/<agent-name>/` | 否（git-ignored） | 否 | 个人项目特定知识 | 🤖 自动运行 — 代理自动读写 |

### 工作原理

1. **启动时**: `MEMORY.md`的前200行被注入代理的系统提示
2. **工具访问**: `Read`、`Write`、`Edit`自动启用以便代理管理其内存
3. **执行期间**: 代理自由读写其内存目录
4. **整理**: 如果`MEMORY.md`超过200行，代理将细节移动到主题特定文件

```
~/.claude/agent-memory/code-reviewer/     # user作用域示例
├── MEMORY.md                              # 主文件（加载前200行）
├── react-patterns.md                      # 主题特定文件
└── security-checklist.md                  # 主题特定文件
```

### Agent Memory vs 其他内存系统

| 系统 | 谁写入 | 谁读取 | 作用域 | 运行方式 |
|------|--------|--------|--------|---------|
| **CLAUDE.md** | 你（手动） | 主Claude + 所有代理 | 项目 | 👆 用户指挥 — 手动写入 |
| **Auto-memory** | 主Claude（自动） | 仅主Claude | 每个项目每个用户 | 🤖 自动运行 — Claude自动学习 |
| **`/memory`命令** | 你（通过编辑器） | 仅主Claude | 每个项目每个用户 | 👆 用户指挥 — 通过 `/memory` 调用 |
| **Agent memory** | 代理本身 | 仅该特定代理 | 可配置（user/project/local） | 🤖 自动运行 — 代理自主管理 |

### 实用示例

```yaml
---
name: api-developer
description: Implement API endpoints following team conventions
tools: Read, Write, Edit, Bash
model: sonnet
memory: project
skills:
  - api-conventions
  - error-handling-patterns
---

Implement API endpoints. Follow the conventions from your preloaded skills.
As you work, save architectural decisions and patterns to your memory.
```

这结合了**skills**（启动时的静态知识）和**memory**（随时间构建的动态知识）。

### 提示

- **提示内存使用** — 包含明确指令：*"Before starting, review your memory. After completing, update your memory with what you learned."*
- **调用代理时请求内存检查**：*"Review this PR, and check your memory for patterns you've seen before."*
- **选择正确的作用域** — `user`用于跨项目，`project`用于团队共享，`local`用于个人

---

## 参考资源

- [Claude Code Documentation](https://code.claude.com/docs)
- [Anthropic Engineering Blog](https://www.anthropic.com/engineering)
- [Claude Code GitHub](https://github.com/anthropics/claude-code)
- [Official Skills Repository](https://github.com/anthropics/skills/tree/main/skills)
