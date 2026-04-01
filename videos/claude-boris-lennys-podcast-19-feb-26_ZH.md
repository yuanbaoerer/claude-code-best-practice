# Claude Code 解决编码之后会发生什么 — Lenny's Podcast

Boris Cherny（[@bcherny](https://x.com/bcherny)）访谈文字稿，Claude Code 创造者，Lenny's Podcast，2026年2月19日发布。

<table width="100%">
<tr>
<td><a href="../">← 返回 Claude Code 最佳实践</a></td>
<td align="right"><img src="../!/claude-jumping.svg" alt="Claude" width="60" /></td>
</tr>
</table>

---

## 视频详情

- **嘉宾：** Boris Cherny（Claude Code 创造者）
- **主持：** Lenny Rachitsky
- **发布：** 2026年2月19日
- **YouTube：** [在 YouTube 观看](https://youtu.be/We7BZVKbCVw)

---

## 主要主题

### 1. Claude Code 的诞生故事

- Boris 加入 Anthropic 时第一个 PR 被拒绝——因为他是手写的
- 第一个原型只是一个终端里的 API 测试工具
- 当他给模型添加 bash 工具，模型自己写 AppleScript 查找他正在听的音乐
- 这是 Boris 的第一次"AGI 时刻"

### 2. 为未来模型构建

Anthropic 的核心理念：
- 不要为今天的模型构建，要为 6 个月后的模型构建
- 早期 Claude Code 不能真正写代码，但团队知道模型会变好
- Sonnet 3.5 和 Opus 4 发布后，产品开始工作

### 3. Boris 的日常工作流

- 每天提交 20-30 个 PR，**零手写代码**
- 卸载了 IDE，100% 使用 Opus 4.5 和 Claude Code
- 80% 的会话从 Plan Mode 开始
- 多个终端标签页并行运行

### 4. CLAUDE.md 设计哲学

Boris 的个人 CLAUDE.md 只有两行：
- 提 PR 时启用 automerge
- 提 PR 时发到内部 stamps 频道

建议：如果太长，删除重新开始。每个模型版本需要添加的内容越来越少。

### 5. 潜在需求（Latent Demand）

Boris 称这是产品中最重要的原则：

- 你无法让人们做他们还没做的事情
- 找到用户已有的意图，然后引导他们更好地利用
- Facebook Dating 成功是因为 60% 的个人资料浏览是异性之间非朋友
- Marketplace 成功是因为 40% 的群组帖子是买卖东西

### 6. Claude Code 团队的工作方式

- 没有 PRD
- 没有强制性的工单系统
- 设计师、数据科学家、财务人员都在写代码
- 发布功能前构建几十甚至上百个原型
- "货币"是演示，不是文档

### 7. Agent Teams 和并行化

- 不相关的上下文窗口是 test-time compute 的一种形式
- Plugins 功能完全由 agent swarm 在周末完成
- 调试时可以用多个 sub-agent 并行查找

### 8. 招聘新标准

团队呈现双峰分布：
- **超专家：** 深度理解某个领域（如 dev tools、JavaScript runtime）
- **超通才：** 跨越产品、设计、用户研究、业务

面试问题："你什么时候错了？"——测试候选人的科学思维和学习能力

### 9. 产品迭代文化

- 每个功能有多个原型
- 终端 spinner 经过 50-100 次迭代
- 用 Claude Code 可以在几小时内测试 20 个原型

### 10. Co-work 的诞生

- 内部发现：财务团队、设计师都在跳过障碍安装终端来用 Claude Code
- Co-work 就是 Claude Code 包装在桌面应用 GUI 里
- 10 天完成，100% 由 Claude Code 编写

### 11. Lightning Round 推荐

**书籍：**
- *Functional Programming in Scala* — 最好的技术书，教会类型思维
- *Accelerando* by Charles Stross — 捕捉当前时刻的节奏
- *流浪地球* by 刘慈欣 — 短篇故事集，中文科幻的独特视角

**产品：**
- Co-work — Chrome 集成，自动处理交通罚单、取消订阅
- Acquired 播客 — Nintendo 集是入门推荐

**人生格言：** 使用常识。很多人失败是因为盲从流程而不思考。

---

## 关键引用

> "不要为今天的模型构建。要为 6 个月后的模型构建。"

> "我卸载了我的 IDE。我不再手动编辑任何一行代码。"

> "潜在需求是产品中最重要的原则——你无法让人们做他们还没做的事情。"

> "我的错误率大概是一半。一半的想法是坏的，你只能尝试。"

> "使用常识。很多人失败是因为盲从流程而不思考。"

---

## 来源

- [Head of Claude Code: What Happens After Coding Is Solved — Lenny's Podcast — YouTube](https://youtu.be/We7BZVKbCVw)
- [Lenny's Podcast](https://www.lennyspodcast.com/)