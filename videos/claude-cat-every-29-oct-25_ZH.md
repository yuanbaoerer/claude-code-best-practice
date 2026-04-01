# Claude Code 工程师揭秘内部故事 — Every

Boris Cherny（[@bcherny](https://x.com/bcherny)）和 Cat Chine（[@_cat_chine](https://x.com/_cat_chine)）访谈文字稿，Claude Code 团队工程师，Every 频道，2025年10月29日发布。

<table width="100%">
<tr>
<td><a href="../">← 返回 Claude Code 最佳实践</a></td>
<td align="right"><img src="../!/claude-jumping.svg" alt="Claude" width="60" /></td>
</tr>
</table>

---

## 视频详情

- **嘉宾：** Boris Cherny & Cat Chine（Claude Code 团队工程师）
- **主持：** Dan Shipper（Every）
- **发布：** 2025年10月29日
- **YouTube：** [在 YouTube 观看](https://youtu.be/IDSAMqip6ms)

---

## 主要主题

### 1. Claude Code 的诞生

- Boris 加入 Anthropic 时，第一个 PR 被拒绝——因为他是手写的
- 第一个原型只是一个终端里的 API 测试工具，没有 UI
- 当他给模型添加 bash 工具，模型自己写 AppleScript 查找他正在听的音乐
- 这是 Boris 的第一次"AGI 时刻"——模型只想使用工具

### 2. 为未来模型构建

Anthropic 的核心理念：
- 不要为今天的模型构建，要为 6 个月后的模型构建
- 早期 Claude Code 不能真正写代码，但团队知道模型会变好
- 随着模型提升，脚手架代码被删除重写

### 3. 潜在需求驱动产品

Boris 称这是产品中最重要的原则：

- Plan Mode 来自用户行为——用户在聊天中让 Claude 规划但不写代码
- CLAUDE.md 来自用户写 markdown 文件让模型读取
- Co-work 来自非技术用户跳过障碍安装终端

### 4. Plan Mode 的设计

- 本质上只是在提示词中加一句"请先不要写代码"
- 可以显著提升成功率（2-3x）
- 常见错误：新用户不使用 Plan Mode

### 5. 终端的设计语言

团队在发现终端的新 UX 规范：
- Shift+Tab 用于 auto-accept
- Tab 用于切换 thinking mode
- 每个交互经过 50-100 次迭代
- Progressive disclosure——复杂功能只在需要时显示

### 6. Power User 建议

- Plan Mode 中让 Claude 提问，像头脑风暴伙伴
- 将 settings.json 提交到代码库，预授权命令
- Stop hooks 可以让模型继续直到测试通过

### 7. 终端 vs GUI

- Boris 曾以为终端只有三个月寿命
- VS Code GUI 扩展让非技术用户更容易使用
- 未来可能有更多形态——模型自主运行时间已达双位数小时

### 8. 产品迭代文化

- 从文档转向演示——内部"货币"是演示
- Boris 称最喜欢红色 diff——删除代码
- 每个功能有多个原型迭代

### 9. Anthropic 内部的使用

- 团队规模翻倍，但人均生产力增长近 70%
- 数据科学家、设计师都在使用 Claude Code
- 简单任务不需要模型"想"五分钟

### 10. Claude Code 的成长

- 产品完全由用户驱动改进
- 70% 的初创公司选择 Claude 作为首选模型
- NASA 用它为火星探测器规划路线

---

## 关键引用

> "不要为今天的模型构建。要为 6 个月后的模型构建。"

> "模型只想使用工具。这就是我第一次 AGI 时刻。"

> "潜在需求是产品中最重要的原则。"

> "Plan Mode 本质上只是在提示词中加一句'请先不要写代码'。"

> "我最喜欢的 diff 是红色的——删除代码。"

---

## 来源

- [The Secrets of Claude Code From the Engineers Who Built It — Every — YouTube](https://youtu.be/IDSAMqip6ms)
- [Every](https://every.to/)