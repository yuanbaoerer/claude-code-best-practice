# Claude 高级工具使用模式

API 级功能（现已 GA），可减少 token 消耗、延迟并提高工具准确性。随 Opus/Sonnet 4.6 发布。

<table width="100%">
<tr>
<td><a href="../">← 返回 Claude Code 最佳实践</a></td>
<td align="right"><img src="../!/claude-jumping.svg" alt="Claude" width="60" /></td>
</tr>
</table>

## 目录

1. [概述](#概述)
2. [程序化工具调用 (PTC)](#程序化工具调用-ptc)
3. [Web 搜索/获取的动态过滤](#web-搜索获取的动态过滤)
4. [工具搜索工具](#工具搜索工具)
5. [工具使用示例](#工具使用示例)
6. [Claude Code 相关性](#claude-code-相关性)

---

## 概述

| 功能 | 解决的问题 | Token 节省 | 可用性 |
|---------|---------------|---------------|--------------|
| 程序化工具调用 | 多步骤代理循环在往返中消耗 token | ~37% 减少 | API、Foundry (GA) |
| 动态过滤 | Web 搜索/获取结果用无关内容膨胀上下文 | 输入 token 减少 ~24% | API、Foundry (GA) |
| 工具搜索工具 | 太多工具定义膨胀上下文 | ~85% 减少 | API、Foundry (GA) |
| 工具使用示例 | 模式本身无法表达使用模式 | 72% → 90% 准确率 | API、Foundry (GA) |

所有功能自 **2026 年 2 月 18 日起正式可用**。

**战略分层** — 从最大的瓶颈开始：
- 工具定义导致的上下文膨胀 → 工具搜索工具
- 大型中间结果 → 程序化工具调用
- Web 搜索噪声 → 动态过滤
- 参数错误 → 工具使用示例

---

## 程序化工具调用 (PTC)

<img src="assets/programmatic-tool-calling-diagram.svg" alt="PTC 图 — 传统 vs 程序化工具调用" width="100%" />

### 范式转变

**之前（传统工具调用）：**
```
用户提示 → Claude → 工具调用 1 → 响应 1 → Claude → 工具调用 2 → 响应 2 → Claude → 工具调用 3 → 响应 3 → Claude → 最终答案
```
每个工具调用需要完整的模型往返。3 个工具 = 3 次推理传递。

**之后（程序化工具调用）：**
```
用户提示 → Claude → 编写 Python 脚本 → 脚本内部调用工具 1、工具 2、工具 3 → stdout → Claude → 最终答案
```
Claude 编写协调所有工具的代码。只有最终的 `stdout` 进入上下文窗口。3 个工具 = 1 次推理传递。

### 工作原理

1. 你使用 `allowed_callers: ["code_execution_20250825"]` 定义工具
2. Claude 编写在沙箱内作为异步函数调用这些工具的 Python 代码
3. 当工具函数被调用时，沙箱暂停，API 返回 `tool_use` 块
4. 你提供工具结果 — 它进入**运行中的代码**，而不是 Claude 的上下文
5. 代码恢复，处理结果，如果需要调用更多工具
6. 只有最终执行的 `stdout` 到达 Claude

### 关键配置

```json
{
  "tools": [
    {
      "type": "code_execution_20250825",
      "name": "code_execution"
    },
    {
      "name": "query_database",
      "description": "执行 SQL 查询。返回 JSON 对象行，字段：id (str)、name (str)、revenue (float)。",
      "input_schema": {
        "type": "object",
        "properties": {
          "sql": { "type": "string", "description": "要执行的 SQL 查询" }
        },
        "required": ["sql"]
      },
      "allowed_callers": ["code_execution_20250825"]
    }
  ]
}
```

### `allowed_callers` 字段

| 值 | 行为 |
|-------|----------|
| `["direct"]` | 仅传统工具调用（省略时的默认值） |
| `["code_execution_20250825"]` | 仅可从 Python 沙箱调用 |
| `["direct", "code_execution_20250825"]` | 两种模式都可用 |

**推荐：** 每个工具选择一种模式，而不是两种。这给 Claude 更清晰的指导。

### 响应中的 `caller` 字段

每个工具使用块包含 `caller` 字段，让你知道它是如何被调用的：

```json
// 直接（传统）
{ "caller": { "type": "direct" } }

// 程序化（从代码执行）
{ "caller": { "type": "code_execution_20250825", "tool_id": "srvtoolu_abc123" } }
```

### 高级模式

**批处理** — 在 1 次推理传递中处理 N 个项目：
```python
regions = ["West", "East", "Central", "North", "South"]
results = {}
for region in regions:
    data = await query_database(f"SELECT SUM(revenue) FROM sales WHERE region='{region}'")
    results[region] = data[0]["revenue"]

top = max(results.items(), key=lambda x: x[1])
print(f"Top region: {top[0]} with ${top[1]:,}")
```

**提前终止** — 成功标准满足后立即停止：
```python
endpoints = ["us-east", "eu-west", "apac"]
for endpoint in endpoints:
    status = await check_health(endpoint)
    if status == "healthy":
        print(f"Found healthy endpoint: {endpoint}")
        break
```

**条件工具选择：**
```python
file_info = await get_file_info(path)
if file_info["size"] < 10000:
    content = await read_full_file(path)
else:
    content = await read_file_summary(path)
print(content)
```

**数据过滤** — 减少 Claude 看到的内容：
```python
logs = await fetch_logs(server_id)
errors = [log for log in logs if "ERROR" in log]
print(f"Found {len(errors)} errors")
for error in errors[-10:]:
    print(error)
```

### 模型兼容性

| 模型 | 支持 |
|-------|-----------|
| Claude Opus 4.6 | 是 |
| Claude Sonnet 4.6 | 是 |
| Claude Sonnet 4.5 | 是 |
| Claude Opus 4.5 | 是 |

### 约束

| 约束 | 详情 |
|-----------|--------|
| **不在 Bedrock/Vertex 上** | 仅 API 和 Foundry |
| **无 MCP 工具** | MCP 连接器工具不能程序化调用 |
| **无 web 搜索/获取** | Web 工具在 PTC 中不支持 |
| **无结构化输出** | `strict: true` 工具不兼容 |
| **无强制工具选择** | `tool_choice` 不能强制 PTC |
| **容器生命周期** | ~4.5 分钟后过期 |
| **ZDR** | 不受零数据保留覆盖 |
| **工具结果为字符串** | 验证外部结果的代码注入风险 |

### 何时使用 PTC

| 良好用例 | 不太理想 |
|----------------|------------|
| 处理需要聚合的大型数据集 | 简单响应的单工具调用 |
| 3+ 个依赖工具按顺序调用 | 需要立即用户反馈的工具 |
| 在 Claude 看到之前过滤/转换结果 | 非常快的操作（开销 > 收益） |
| 跨许多项目的并行操作 | |
| 基于中间结果的条件逻辑 | |

### Token 效率

- 程序化调用的工具结果**不添加到 Claude 的上下文** — 只有最终 `stdout`
- 中间处理在代码中进行，不是模型 token
- 10 个程序化工具 ≈ 10 个直接调用的 1/10 token

---

## Web 搜索/获取的动态过滤

### 问题

Web 搜索和获取工具将完整 HTML 页面转储到 Claude 的上下文窗口。这些内容大多不相关 — 导航、广告、样板文字。Claude 然后对所有内容进行推理，浪费 token 并降低准确性。

### 解决方案

Claude 现在**编写并执行 Python 代码在 Web 结果进入上下文窗口之前过滤它们**。Claude 在沙箱中过滤、解析并仅提取相关内容，而不是对原始 HTML 进行推理。

### 工作原理

**之前：**
```
查询 → 搜索结果 → 获取完整 HTML × N 页 → 所有内容进入上下文 → Claude 对所有内容推理
```

**之后：**
```
查询 → 搜索结果 → Claude 编写过滤代码 → 代码仅提取相关内容 → 过滤后的结果进入上下文
```

### API 配置

使用带有 beta 头部的更新工具类型版本：

```json
{
  "model": "claude-opus-4-6",
  "max_tokens": 4096,
  "tools": [
    {
      "type": "web_search_20260209",
      "name": "web_search"
    },
    {
      "type": "web_fetch_20260209",
      "name": "web_fetch"
    }
  ]
}
```

**需要的头部：** `anthropic-beta: code-execution-web-tools-2026-02-09`

使用新工具类型版本时，Sonnet 4.6 和 Opus 4.6 **默认启用**。

### 基准结果

**BrowseComp**（在网站上查找特定信息）：

| 模型 | 无过滤 | 有过滤 | 改进 |
|-------|-------------------|----------------|-------------|
| Sonnet 4.6 | 33.3% | **46.6%** | +13.3 pp |
| Opus 4.6 | 45.3% | **61.6%** | +16.3 pp |

**DeepsearchQA**（多步骤研究，F1 分数）：

| 模型 | 无过滤 | 有过滤 | 改进 |
|-------|-------------------|----------------|-------------|
| Sonnet 4.6 | 52.6% | **59.4%** | +6.8 pp |
| Opus 4.6 | 69.8% | **77.3%** | +7.5 pp |

**Token 效率：** 平均减少 24% 输入 token。Sonnet 4.6 看到成本降低；Opus 4.6 可能因更复杂的过滤代码略有增加。

### 用例

- 筛技术文档
- 跨多个来源验证引用
- 交叉引用搜索结果
- 多步骤研究查询
- 查找埋藏在大页面中的特定数据点

---

## 工具搜索工具

### 问题

预先加载所有工具定义浪费上下文。如果你有 50 个 MCP 工具，每个约 1.5K token，那是 75K token，在用户提问之前就消耗了。

### 解决方案

用 `defer_loading: true` 标记不常用的工具。它们被排除在初始上下文之外。Claude 通过工具搜索工具按需发现它们。

### 配置

```json
{
  "tools": [
    {
      "type": "mcp_toolset",
      "mcp_server_name": "google-drive",
      "default_config": { "defer_loading": true },
      "configs": {
        "search_files": { "defer_loading": false }
      }
    }
  ]
}
```

### 最佳实践

- 保持 3-5 个最常用的工具始终加载，延迟其余
- 编写清晰、描述性的工具名称和描述（搜索依赖它们）
- 在系统提示中记录可用功能

### 何时使用

- 工具定义消耗 > 10K token
- 有 10+ 个可用工具
- 多个 MCP 服务器
- 工具选择准确性因选项太多而出问题

### Token 节省

工具定义 token 减少约 85%（Anthropic 基准中 77K → 8.7K）。

### Claude Code 等效

Claude Code 有 **MCP 工具搜索自动模式**（自 v2.1.7 起默认启用）。当 MCP 工具描述超过上下文的 10% 时，它们被延迟并通过 `MCPSearch` 发现。用 `ENABLE_TOOL_SEARCH=auto:N` 配置阈值，其中 N 是上下文百分比（0-100）。

---

## 工具使用示例

### 问题

JSON 模式定义结构但无法表达：
- 何时包含可选参数
- 哪些参数组合有意义
- 格式约定（日期格式、ID 模式）
- 嵌套结构使用

### 解决方案

在工具定义中添加 `input_examples` — 模式之外的具体使用模式。

### 配置

```json
{
  "name": "create_ticket",
  "description": "创建支持工单",
  "input_schema": {
    "type": "object",
    "properties": {
      "title": { "type": "string" },
      "priority": { "type": "string", "enum": ["low", "medium", "high", "critical"] },
      "assignee": { "type": "string" },
      "labels": { "type": "array", "items": { "type": "string" } }
    },
    "required": ["title"]
  },
  "input_examples": [
    {
      "title": "登录页面返回 500 错误",
      "priority": "critical",
      "assignee": "oncall-team",
      "labels": ["bug", "auth", "production"]
    },
    {
      "title": "添加深色模式支持",
      "priority": "low",
      "labels": ["feature-request", "ui"]
    },
    {
      "title": "更新 v2 端点的 API 文档"
    }
  ]
}
```

### 最佳实践

- 使用**真实数据**，不是像 "example_value" 这样的占位符字符串
- 展示**多样性**：最小、部分和完整规范
- 保持简洁：**每个工具 1-5 个示例**
- 专注于消除歧义 — 目标是行为清晰而非模式完整性
- 展示参数相关性（如 `priority: "critical"` 通常有 `assignee`）

### 结果

Anthropic 基准中复杂参数处理的准确率从 72% 提高到 90%。

---

## Claude Code 相关性

### 什么直接适用于 Claude Code 用户

| 功能 | Claude Code 状态 | 操作 |
|---------|-------------------|--------|
| 工具搜索 | 自 v2.1.7 起内置为 MCPSearch 自动模式 | 如果有很多 MCP 工具，调整 `ENABLE_TOOL_SEARCH=auto:N` |
| 动态过滤 | 在 CLI 中不可用（API 级 web 工具） | 与做 web 研究的 Agent SDK 用户相关 |
| PTC | 在 CLI 中不可用 | 与构建自定义代理的 Agent SDK 用户相关 |
| 工具使用示例 | 在 CLI 中不可配置 | 与自定义 MCP 服务器作者相关 |

### 对于 Agent SDK 开发者

如果你使用 `@anthropic-ai/claude-agent-sdk` 构建代理，PTC 立即可用：

1. 添加 `code_execution_20250825` 到你的 tools 数组
2. 在受益于批处理/过滤的工具上设置 `allowed_callers`
3. 实现工具结果循环（暂停 → 提供结果 → 恢复）
4. 从工具返回结构化数据（JSON）以便更容易的程序化解析

### 对于 MCP 服务器作者

如果你构建自定义 MCP 服务器，工具使用示例可以改善 Claude 使用你工具的方式：
- 添加 `input_examples` 到工具模式
- 在描述中清楚地记录返回格式（PTC 需要解析它们）

---

## 来源

- [Anthropic Engineering: 高级工具使用](https://www.anthropic.com/engineering/advanced-tool-use)
- [程序化工具调用文档](https://platform.claude.com/docs/en/agents-and-tools/tool-use/programmatic-tool-calling)
- [代码执行工具文档](https://platform.claude.com/docs/en/agents-and-tools/tool-use/code-execution-tool)
- [通过动态过滤改进 Web 搜索](https://claude.com/blog/improved-web-search-with-dynamic-filtering)