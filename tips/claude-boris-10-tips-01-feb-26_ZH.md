# 使用 Claude Code 的 10 个技巧 —— 来自 Claude Code 团队

Boris Cherny ([@bcherny](https://x.com/bcherny))，Claude Code 的创造者，于 2026 年 2 月 1 日分享的团队技巧总结。

<table width="100%">
<tr>
<td><a href="../">← 返回 Claude Code 最佳实践</a></td>
<td align="right"><img src="../!/claude-jumping.svg" alt="Claude" width="60" /></td>
</tr>
</table>

---

## 背景

Boris 分享了直接来自 Claude Code 团队的使用 Claude Code 技巧。团队使用 Claude 的方式与 Boris 个人使用方式不同。记住：没有一种正确的使用 Claude Code 的方式 —— 每个人的设置都不同。你应该实验看看什么适合你！

<a href="https://x.com/bcherny/status/2017742741636321619"><img src="assets/boris-1-feb-26/0.png" alt="Boris Cherny 介绍推文" width="50%" /></a>

---

## 1/ 更多地并行工作

一次启动 3-5 个 git worktrees，每个运行自己的 Claude 会话并行。这是最大的生产力解锁，也是团队的首要技巧。个人上，Boris 使用多个 git checkout，但大多数 Claude Code 团队更喜欢 worktrees —— 这就是 `@amorisscode` 在 Claude 桌面应用中构建原生支持的原因！

有些人还命名他们的 worktrees 并设置 shell 别名（`2a`、`2b`、`2c`）以便一键跳转。其他人有一个专用的"分析" worktree 只用于读取日志和运行 BigQuery。

参见：[Worktrees 文档](https://code.claude.com/docs/en/common...)

<a href="https://x.com/bcherny/status/2017742743125299476"><img src="assets/boris-1-feb-26/1.png" alt="更多地并行工作" width="50%" /></a>

---

## 2/ 每个复杂任务从计划模式开始

将你的精力投入到计划中，以便 Claude 可以一次完成实现。

一个人让一个 Claude 写计划，然后他们启动第二个 Claude 作为高级工程师审查它。

另一个人说一旦出现问题，他们就切换回计划模式并重新计划。不要继续推。他们还显式告诉 Claude 进入计划模式进行验证步骤，不只是构建。

<a href="https://x.com/bcherny/status/2017742745365057733"><img src="assets/boris-1-feb-26/2.png" alt="每个复杂任务从计划模式开始" width="50%" /></a>

---

## 3/ 投入你的 CLAUDE.md

每次纠正后，以："更新你的 CLAUDE.md 这样你不会再犯那个错误"结束。Claude 为自己编写规则异常出色。

随时间无情地编辑你的 `CLAUDE.md`。持续迭代直到 Claude 的错误率可测量地下降。

一位工程师告诉 Claude 为每个任务/项目维护一个笔记目录，每次 PR 后更新。他们然后将 `CLAUDE.md` 指向它。

<a href="https://x.com/bcherny/status/2017742747067945390"><img src="assets/boris-1-feb-26/3.png" alt="投入你的 CLAUDE.md" width="50%" /></a>

---

## 4/ 创建你自己的技能并检入 Git

跨每个项目重用。来自团队的技巧：

- 如果你每天做某事超过一次，将其转换为技能或命令
- 构建一个 `/techdebt` 斜杠命令并在每次会话结束时运行它来发现和消灭重复代码
- 设置一个斜杠命令将 7 天的 Slack、GDrive、Asana 和 GitHub 同步到一个上下文转储
- 构建分析工程师风格的代理，编写 dbt 模型、审查代码并在 dev 中测试更改

参见：[用技能扩展 Claude — Claude Code 文档](https://code.claude.com/docs/en/skills)

<a href="https://x.com/bcherny/status/2017742748984742078"><img src="assets/boris-1-feb-26/4.png" alt="创建你自己的技能" width="50%" /></a>

---

## 5/ Claude 自己修复大多数 Bug

团队这样做：

启用 Slack MCP，然后将 Slack bug 线程粘贴到 Claude 并只需说"修复"。无需上下文切换。

或者，只需说"去修复失败的 CI 测试。"不要微观管理如何。

将 Claude 指向 docker 日志来排查分布式系统 —— 它在这方面惊人地有能力。

<a href="https://x.com/bcherny/status/2017742750473720121"><img src="assets/boris-1-feb-26/5.png" alt="Claude 自己修复大多数 Bug" width="50%" /></a>

---

## 6/ 提升你的提示能力

a. **挑战 Claude。** 说"对我这些更改进行质询，直到我通过你的测试才做 PR。"让 Claude 成为你的审查者。或者，说"向我证明这有效"并让 Claude 在 main 和你的功能分支之间 diff 行为。

b. **在一个平庸的修复后，** 说："知道你现在知道的一切，废弃这个并实现优雅的解决方案。"

c. **编写详细规格**并在移交工作前减少歧义。你越具体，输出越好。

<a href="https://x.com/bcherny/status/2017742752566632544"><img src="assets/boris-1-feb-26/6.png" alt="提升你的提示能力" width="50%" /></a>

---

## 7/ 终端与环境设置

团队喜欢 Ghostty！多人喜欢它的同步渲染、24 位颜色和正确的 unicode 支持。

为了更轻松地处理 Claude，使用 `/statusline` 自定义你的状态栏，始终显示上下文使用和当前 git 分支。许多人还颜色编码和命名他们的终端标签页，有时使用 tmux —— 每个任务/worktree 一个标签页。

使用语音听写。你说话比打字快 3 倍，你的提示因此变得更详细。（在 macOS 上按 fn 两次）

参见：[终端设置文档](https://code.claude.com/docs/en/termin...)

<a href="https://x.com/bcherny/status/2017742753971769626"><img src="assets/boris-1-feb-26/7.png" alt="终端和环境设置" width="50%" /></a>

---

## 8/ 使用子代理

a. 在任何你想让 Claude 对问题投入更多计算的请求后附加"使用子代理"。

b. 将单个任务交给子代理以保持你的主代理上下文窗口清洁和专注。

c. 通过钩子将权限请求路由到 Opus 4.5 — 让它扫描攻击并自动批准安全的那些。参见：[钩子文档](https://code.claude.com/docs/en/hooks#...)

<a href="https://x.com/bcherny/status/2017742755737555434"><img src="assets/boris-1-feb-26/8.png" alt="使用子代理" width="50%" /></a>

---

## 9/ 使用 Claude 进行数据与分析

让 Claude Code 使用 "bq" CLI 动态提取和分析指标。团队有一个 BigQuery 技能检入代码库，每个人都直接在 Claude Code 中使用它进行分析查询。个人上，Boris 6+ 个月没有写一行 SQL。

这适用于任何有 CLI、MCP 或 API 的数据库。

<a href="https://x.com/bcherny/status/2017742757666902374"><img src="assets/boris-1-feb-26/9.png" alt="使用 Claude 进行数据和分析" width="50%" /></a>

---

## 10/ 与 Claude 学习

来自团队使用 Claude Code 学习的几个技巧：

a. 在 `/config` 中启用"解释性"或"学习"输出风格，让 Claude 解释其更改背后的"原因"。

b. 让 Claude 生成一个视觉 HTML 演示解释陌生代码。它制作出惊人的幻灯片！

c. 让 Claude 绘制新协议和代码库的 ASCII 图来帮助你理解它们。

d. 构建间隔重复学习技能：你解释你的理解，Claude 提问后续来填补空白，存储结果。

<a href="https://x.com/bcherny/status/2017742759218794768"><img src="assets/boris-1-feb-26/10.png" alt="与 Claude 学习" width="50%" /></a>

---

## 来源

- [Boris Cherny (@bcherny) 在 X 上 — 2026 年 2 月 1 日](https://x.com/bcherny/status/2017742741636321619)