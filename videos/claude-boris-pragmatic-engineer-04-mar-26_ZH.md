# 与 Boris Cherny 构建 Claude Code — The Pragmatic Engineer

Boris Cherny（[@bcherny](https://x.com/bcherny)）访谈文字稿，Claude Code 创造者，The Pragmatic Engineer 播客，2026年3月4日发布。

<table width="100%">
<tr>
<td><a href="../">← 返回 Claude Code 最佳实践</a></td>
<td align="right"><img src="../!/claude-jumping.svg" alt="Claude" width="60" /></td>
</tr>
</table>

---

## 视频详情

- **嘉宾：** Boris Cherny（Claude Code 创造者）
- **主持：** Gergely Orosz（The Pragmatic Engineer）
- **发布：** 2026年3月4日
- **YouTube：** [在 YouTube 观看](https://youtu.be/julbw1JuAz0)

---

## 主要主题

### 1. 印刷术的比喻

Boris 用 15 世纪印刷术的发明来比喻我们正处于的时刻：

- 当时有一群抄写员知道如何书写，这是一种精英技能
- 一些雇佣他们的国王甚至不识字
- 印刷术出现后，印刷材料成本下降约 100 倍
- 印刷品数量在 50-100 年内增长了 10,000 倍
- 抄写员没有消失——他们变成了作家和作者
- 文学市场大幅扩展，无人能预测

**核心洞见：** 软件工程师就像中世纪的抄写员，我们花了多年掌握这门手艺。现在印刷术（AI）正在到来。但我们不会消失——我们将成为这个新时代的"作家和作者"。

### 2. Claude Code 的诞生

- Boris 加入 Anthropic 时，他的第一个 PR 被拒绝了——不是因为代码不好，而是因为他是手写的
- Claude Code 的理念：**为 6 个月后的模型构建，而非今天的模型**
- 早期 Claude Code 不是一个好产品，但随着 Sonnet 和 Opus 4 的发布，产品开始工作了
- 现在 Claude Code 团队 80-90% 的代码由 Claude Code 自己编写

### 3. Boris 的日常工作流

- 每天提交 20-30 个 PR，**零手写代码**
- 每天写 10-20 个 PR，Opus 4.5 和 Claude Code 写了 100%，他一行都没手动编辑
- 早上醒来打开 Claude Code，启动几个 agent 开始一天的工作
- 有时代码看起来好就直接合并，有时拉到本地用 teleport 编辑

### 4. 代码审查如何工作

当 AI 写所有代码时，代码审查如何工作：

- Claude Code 审查自己的代码
- 自动 lint 规则
- Best-of-N 通行
- 人工代码审查
- **相同的标准**：无论代码是人写的还是模型写的，审查标准完全一样

### 5. 团队工作方式

Claude Code 团队的不同之处：

- 没有 PRD
- 没有强制性的工单系统
- 设计师、数据科学家、财务人员都在写代码
- 发布功能前构建几十甚至上百个原型

### 6. 哪些技能仍然重要

**仍然重要的技能：**
- 系统性和假设驱动的方法论
- 好奇心和跨领域工作的开放性
- 调试能力（仍然需要，但可能 6 个月后就不需要了）
- 适应能力

**不再重要的技能：**
- 对代码风格和语言的强烈观点
- 无尽的语言辩论和框架辩论——模型可以用任何语言和框架，如果你不喜欢它可以重写

**新时代奖励的特质：**
- 通才——跨越工程、产品和业务思考
- 短注意力跨度——工作是管理多个 Claude，在上下文之间快速切换
- Boris 称这是 **"ADHD 之年"**

### 7. 安全的重要性

Boris 表示他从去年到今年信念最大的变化是对 AI 安全问题的担忧：

- 加入 Anthropic 是因为读了大量科幻小说，知道情况可能变得多糟
- 从内部看到过去一年出现的新风险，让他更加担心
- 现在最重要的事情是确保这一切顺利进行

### 8. 推荐书籍

**科幻类：**
- 刘慈欣的短篇故事集（《三体》作者的其他作品）
- *Accelerondo* by Charles Stross — "未来 50 年的产品路线图"，描述了 AI 奇点和加速变化的感觉

**技术类：**
- *Functional Programming in Scala* — 即使语言选择不再那么重要，函数式编程教会你如何更好地编码，如何用类型思考

---

## 关键引用

> "印刷术出现时，抄写员没有消失。他们变成了作家和作者，整个文学市场扩展到任何人无法预测的程度。"

> "不要为今天的模型构建。要为 6 个月后的模型构建。"

> "我认为这将是通才之年。下一个十亿美元的产品可能只是一个人有一些酷想法，他们的大脑能够跨越工程、产品和业务思考。"

> "工作是管理多个 Claude，在上下文之间快速切换。这不再关于深度工作，而是关于上下文切换。"

---

## 来源

- [Building Claude Code with Boris Cherny — The Pragmatic Engineer — YouTube](https://youtu.be/julbw1JuAz0)
- [The Pragmatic Engineer Podcast](https://pragmaticengineer.com/)