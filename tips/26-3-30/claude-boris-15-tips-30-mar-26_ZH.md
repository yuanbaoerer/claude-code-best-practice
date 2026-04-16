# Claude Code 中 15 个隐藏与未被充分利用的功能 —— 来自 Boris Cherny

Boris Cherny ([@bcherny](https://x.com/bcherny))，Claude Code 的创造者，于 2026 年 3 月 30 日分享的技巧总结。

<table width="100%">
<tr>
<td><a href="../">← 返回 Claude Code 最佳实践</a></td>
<td align="right"><img src="../../!/claude-jumping.svg" alt="Claude" width="60" /></td>
</tr>
</table>

---

## 背景

Boris 分享了他最喜欢的 Claude Code 中隐藏和未被充分利用的一系列功能，专注于他使用最多的那些。

<a href="https://x.com/bcherny/status/2038454336355999749"><img src="../assets/boris-30-mar-26/0.png" alt="Boris Cherny 介绍推文" width="50%" /></a>

---

## 1/ Claude Code 有移动应用

你知道 Claude Code 有移动应用吗？Boris 从 iOS 应用编写了大量代码 —— 这是不打开笔记本电脑就能进行更改的便捷方式。

- 下载 iOS/Android 的 Claude 应用
- 导航到左侧的 **Code** 标签页
- 你可以直接从手机审查更改、批准 PR 和编写代码

<a href="https://x.com/bcherny/status/2038454337811386436"><img src="../assets/boris-30-mar-26/1.png" alt="Claude Code 移动应用" width="50%" /></a>

---

## 2/ 在移动/Web/桌面和终端之间移动会话

运行 `claude --teleport` 或 `/teleport` 在你的机器上继续云端会话。或运行 `/remote-control` 从你的手机/web 控制本地运行的会话。

- **Teleport**：将云端会话拉取到你的本地终端
- **远程控制**：让你从任何设备控制本地会话
- Boris 在他的 `/config` 中设置了 **"为所有会话启用远程控制"**

<a href="https://x.com/bcherny/status/2038454339933548804"><img src="../assets/boris-30-mar-26/2.png" alt="Teleport 和远程控制" width="50%" /></a>

---

## 3/ /loop 和 /schedule —— 两个最强大的功能

使用这些来安排 Claude 以固定间隔自动运行，一次最多一周。Boris 本地运行了许多循环：

- `/loop 5m /babysit` — 自动处理代码审查、自动变基，并将 PR 护送到生产环境
- `/loop 30m /slack-feedback` — 每 30 分钟自动为 Slack 反馈提交 PR
- `/loop /post-merge-sweeper` — 提交 PR 处理他遗漏的代码审查评论
- `/loop 1h /pr-pruner` — 关闭陈旧和不再需要的 PR
- ...还有更多！

尝试将工作流程转换为技能 + 循环。这很强大。

<a href="https://x.com/bcherny/status/2038454341884154269"><img src="../assets/boris-30-mar-26/3.png" alt="/loop 和 /schedule" width="50%" /></a>

---

## 4/ 使用钩子确定性运行逻辑

使用钩子作为代理生命周期的一部分运行逻辑。例如：

- 每次启动 Claude 时**动态加载**上下文 (`SessionStart`)
- **记录模型运行的每个 bash 命令** (`PreToolUse`)
- 将权限提示**路由到 WhatsApp** 让你批准/拒绝 (`PermissionRequest`)
- 每当 Claude 停止时**提醒 Claude** 继续 (`Stop`)

<a href="https://x.com/bcherny/status/2038454343519932844"><img src="../assets/boris-30-mar-26/4.png" alt="使用钩子" width="50%" /></a>

---

## 5/ Cowork Dispatch

Boris 每天使用 Dispatch 来跟进 Slack 和电子邮件、管理文件，并在他不在电脑旁时在他的笔记本电脑上做事。当他不在编码时，他就在 dispatching。

- Dispatch 是 Claude 桌面应用的**安全远程控制**
- 它可以使用你的 MCP、浏览器和计算机，经你许可
- 把它看作从任何地方向 Claude 委派非编码任务的方式

<a href="https://x.com/bcherny/status/2038454345419936040"><img src="../assets/boris-30-mar-26/5.png" alt="Cowork Dispatch" width="50%" /></a>

---

## 6/ 使用 Chrome 扩展进行前端工作

使用 Claude Code 最重要的技巧：**给 Claude 一种验证其输出的方式。** 一旦你这样做，Claude 会迭代直到结果很好。

- 把它想象成让某人构建网站但不允许他们使用浏览器 —— 结果可能不会好看
- 给 Claude 一个浏览器，它会编写代码并迭代直到看起来好看
- Boris 每次在 web 代码上工作时都使用 Chrome 扩展 —— 它往往比其他类似 MCP 工作更可靠

<a href="https://x.com/bcherny/status/2038454347156398333"><img src="../assets/boris-30-mar-26/6.png" alt="前端 Chrome 扩展" width="50%" /></a>

---

## 7/ 使用 Claude 桌面应用自动启动和测试 Web 服务器

同样，桌面应用捆绑了 Claude **自动运行你的 web 服务器甚至在内置浏览器中测试它**的能力。

- 你可以在 CLI 或 VSCode 中使用 Chrome 扩展设置类似的东西
- 或直接使用桌面应用获得集成体验

<a href="https://x.com/bcherny/status/2038454348804714642"><img src="../assets/boris-30-mar-26/7.png" alt="桌面应用 web 服务器测试" width="50%" /></a>

---

## 8/ 分叉你的会话

人们经常问如何分叉现有会话。两种方式：

1. 从你的会话运行 `/branch`
2. 从 CLI，运行 `claude --resume <session-id> --fork-session`

`/branch` 创建一个分支对话 —— 你现在在分支中。要恢复原始会话，使用 `claude -r <original-session-id>`。

<a href="https://x.com/bcherny/status/2038454350214041740"><img src="../assets/boris-30-mar-26/8.png" alt="分叉你的会话" width="50%" /></a>

---

## 9/ 使用 /btw 进行侧边查询

Boris 经常使用这个在代理工作时回答快速问题。`/btw` 让你提问而不中断代理的当前任务。

示例：
```
/btw dachshund 怎么拼写？
> dachshund — 德语 "badger dog"（dachs + 獾，hund + 狗）
↑/↓ 滚动 · 空格、回车或 Escape 关闭
```

<a href="https://x.com/bcherny/status/2038454351849787485"><img src="../assets/boris-30-mar-26/9.png" alt="/btw 侧边查询" width="50%" /></a>

---

## 10/ 使用 Git Worktrees

Claude Code 内置对 git worktrees 的深度支持。Worktrees 对于在同一仓库中进行大量并行工作至关重要。Boris **始终运行数十个 Claude**，这就是他的方式。

- 使用 `claude -w` 在 worktree 中启动新会话
- 或在 Claude 桌面应用中点击 **"worktree" 复选框**
- 对于非 git VCS 用户，使用 `WorktreeCreate` 钩子添加你自己的 worktree 创建逻辑

<a href="https://x.com/bcherny/status/2038454353787519164"><img src="../assets/boris-30-mar-26/10.png" alt="Git worktrees" width="50%" /></a>

---

## 11/ 使用 /batch 分发大型变更集

`/batch` 会询问你，然后让 Claude 将工作分发给所需数量的 **worktree 代理**（数十、数百甚至数千）来完成。

- 用于大型代码迁移和其他类型的可并行化工作
- 每个 worktree 代理在自己的代码副本上独立工作

<a href="https://x.com/bcherny/status/2038454355469484142"><img src="../assets/boris-30-mar-26/11.png" alt="/batch 大型变更集" width="50%" /></a>

---

## 12/ 使用 --bare 将 SDK 启动速度提高多达 10 倍

默认情况下，当你运行 `claude -p`（或 TypeScript 或 Python SDK）时，Claude 会搜索本地 CLAUDE.md、设置和 MCP。但对于非交互式使用，大多数时候你希望通过 `--system-prompt`、`--mcp-config`、`--settings` 等显式指定要加载的内容。

- 这是 SDK 首次构建时的设计疏忽
- 在未来版本中，他们会将默认值翻转为 `--bare`
- 现在，使用该标志获得多达 **10 倍更快的启动**

```bash
claude -p "总结这个代码库" \
    --output-format=stream-json \
    --verbose \
    --bare
```

<a href="https://x.com/bcherny/status/2038454357088457168"><img src="../assets/boris-30-mar-26/12.png" alt="--bare SDK 启动标志" width="50%" /></a>

---

## 13/ 使用 --add-dir 让 Claude 访问更多文件夹

当跨多个仓库工作时，Boris 通常在一个仓库中启动 Claude 并使用 `--add-dir`（或 `/add-dir`）让 Claude 看到另一个仓库。

- 这不仅告诉 Claude 关于仓库，还**给它权限**在仓库中工作
- 或，将 `"additionalDirectories"` 添加到你团队的 `settings.json` 以在启动 Claude Code 时始终加载额外文件夹

<a href="https://x.com/bcherny/status/2038454359047156203"><img src="../assets/boris-30-mar-26/13.png" alt="--add-dir 多仓库" width="50%" /></a>

---

## 14/ 使用 --agent 给 Claude Code 自定义系统提示与工具

自定义代理是一个经常被忽视的强大原语。要使用它，只需在 `.claude/agents/` 中定义一个新代理，然后运行：

```bash
claude --agent=<你的代理名称>
```

- 代理可以有受限工具、自定义描述和特定模型
- 它们非常适合创建只读代理、专门审查代理或领域特定工具

<a href="https://x.com/bcherny/status/2038454360418787764"><img src="../assets/boris-30-mar-26/14.png" alt="--agent 自定义系统提示" width="50%" /></a>

---

## 15/ 使用 /voice 启用语音输入

有趣的事实：Boris 大部分编码是通过说话给 Claude，而不是打字。

- 在 CLI 中运行 `/voice` 然后按住空格键说话
- 在桌面应用中按语音按钮
- 或在你的 iOS 设置中启用听写

<a href="https://x.com/bcherny/status/2038454362226467112"><img src="../assets/boris-30-mar-26/15.png" alt="/voice 语音输入" width="50%" /></a>

---

## 来源

- [Boris Cherny (@bcherny) 在 X 上 — 2026 年 3 月 30 日](https://x.com/bcherny/status/2038454336355999749)