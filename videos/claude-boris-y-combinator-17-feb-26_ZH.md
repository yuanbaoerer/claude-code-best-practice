# Boris Cherny（Claude Code 创造者）谈 Claude Code 内幕 — Y Combinator

Boris Cherny（[@bcherny](https://x.com/bcherny)）访谈文字稿，Claude Code 创造者，Y Combinator Light Cone 播客，2026年2月17日发布。

<table width="100%">
<tr>
<td><a href="../">← 返回 Claude Code 最佳实践</a></td>
<td align="right"><img src="../!/claude-jumping.svg" alt="Claude" width="60" /></td>
</tr>
</table>

---

## 视频详情

- **嘉宾：** Boris Cherny（Claude Code 创造者）
- **主持：** Y Combinator（The Light Cone）
- **发布：** 2026年2月17日
- **YouTube：** [在 YouTube 观看](https://youtu.be/PQU9o_5rHC4)

---

## 主要主题

### 1. 为 6 个月后的模型构建

Boris 强调这是 Anthropic 的核心理念：

- 不要为今天的模型构建产品，要为 6 个月后的模型构建
- Claude Code 早期不能真正写代码，但团队知道模型会变好
- 现在团队 70-90% 的代码由 Claude Code 编写，Boris 个人达到 100%
- 每天提交 20 个 PR，零手写代码

### 2. Claude Code 的诞生

- 第一个原型只是 Boris 在终端里写的一个 API 测试工具
- 给模型添加 bash 工具后，模型自己写了 AppleScript 查找 Boris 正在听的音乐
- 这是 Boris 的第一次"AGI 时刻"——模型只想使用工具
- 团队没有强制推广，但内部使用图表呈垂直增长

### 3. CLAUDE.md 的设计哲学

Boris 的个人 CLAUDE.md 只有两条规则：
- 提 PR 时启用 automerge
- 提 PR 时发到内部团队 stamps 频道

**建议：** 如果 CLAUDE.md 太长，删除它重新开始。每个模型需要添加的内容越来越少。

### 4. Plan Mode 的有限生命周期

- Plan Mode 本质上只是在提示词中加一句"请先不要写代码"
- Boris 估计 Plan Mode 可能一个月后就不再需要了
- 80% 的会话从 Plan Mode 开始
- 用 Opus 4.5，一旦计划好，模型几乎每次都能正确执行

### 5. 招聘工程师的新标准

- **专家 vs 通才：** 团队呈现双峰分布——超专家和超通才
- 通才跨越产品、设计、用户研究、业务思考
- 面试问题："你什么时候错了？"——看候选人能否承认错误并学习

### 6. Agent Teams 架构

- 不相关的上下文窗口——多个 agent 有独立的上下文，不被彼此污染
- 这是 test-time compute 的一种形式
- Plugins 功能完全由 agent swarm 在一个周末完成

### 7. "苦涩的教训"（The Bitter Lesson）

团队墙上挂着 Rich Sutton 的这篇文章：

- 更通用的模型总是胜过更特定的模型
- 不要和模型作对
- 可以现在写脚手架代码提升 10-20%，或等几个月模型自己能做到

### 8. 终端的未来

- Boris 曾以为终端只有三个月寿命
- 现在产品在 Web、桌面应用、iOS、Android、Slack、GitHub 都有
- 代码生命周期现在只有几个月

### 9. 产品开发文化

- 从文档转向演示——内部"货币"是演示
- 一个功能通常有多个原型迭代
- 建立终端的新设计语言，每件事都要迭代几十次

### 10. 安全与使命

- Boris 加入 Anthropic 是因为读科幻小说知道情况可能变得多糟
- 模型可能在今年达到 ASL4（递归自我改进）
- Anthropic 午餐时间人们都在讨论 AI 安全

---

## 关键引用

> "不要为今天的模型构建。要为 6 个月后的模型构建。"

> "Plan Mode 本质上只是在提示词中加一句'请先不要写代码'。"

> "代码的生命周期现在只有几个月。"

> "更通用的模型总是胜过更特定的模型——永远不要和模型作对。"

> "我卸载了我的 IDE。我不再手动编辑任何一行代码。"

---

## 来源

- [Inside Claude Code With Its Creator Boris Cherny — Y Combinator — YouTube](https://youtu.be/PQU9o_5rHC4)
- [Y Combinator](https://www.ycombinator.com/)