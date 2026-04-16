# 我如何使用 Claude Code —— Boris Cherny 的 13 个技巧

Boris Cherny ([@bcherny](https://x.com/bcherny))，Claude Code 的创造者，于 2026 年 1 月 3 日分享的设置技巧总结。

<table width="100%">
<tr>
<td><a href="../">← 返回 Claude Code 最佳实践</a></td>
<td align="right"><img src="../../!/claude-jumping.svg" alt="Claude" width="60" /></td>
</tr>
</table>

---

## 背景

Boris 分享了他个人的 Claude Code 设置，注意到它"令人惊讶地普通" —— Claude Code 开箱即用效果很好，所以他没有太多自定义。没有一种正确的使用方式：团队有意构建它，以便你可以按你喜欢的方式使用、自定义和 hack。Claude Code 团队的每个人使用方式都非常不同。

<a href="https://x.com/bcherny/status/2007179832300581177"><img src="../assets/boris-3-jan-26/0.png" alt="Boris Cherny 介绍推文" width="50%" /></a>

---

## 1/ 并行运行 5 个 Claude

在你的终端中并行运行 5 个 Claude。将你的标签页编号 1-5，并使用系统通知知道何时 Claude 需要输入。

参见：[终端设置文档](https://code.claude.com/docs/en/terminal)

<a href="https://x.com/bcherny/status/2007179833990885678"><img src="../assets/boris-3-jan-26/1.png" alt="并行运行 5 个 Claude" width="50%" /></a>

---

## 2/ 使用 claude.ai/code 获得更多并行性

在 claude.ai/code 上与你的本地 Claude 并行运行 5-10 个 Claude。使用 `claude.ai/code` 将本地会话交给 web 会话，在 Chrome 中手动启动会话，并来回传送。

<a href="https://x.com/bcherny/status/2007179836704600237"><img src="../assets/boris-3-jan-26/2.png" alt="claude.ai/code 并行性" width="50%" /></a>

---

## 3/ 对所有事情使用带思考的 Opus

对所有事情使用带思考的 Opus 4.5。这是 Boris 使用过的最好的编码模型 —— 尽管它比 Sonnet 更大更慢，但由于你需要更少地引导它且它在工具使用方面更好，最终几乎总是比使用更小的模型更快。

<a href="https://x.com/bcherny/status/2007179838864666847"><img src="../assets/boris-3-jan-26/3.png" alt="带思考的 Opus" width="50%" /></a>

---

## 4/ 与你的团队共享单个 CLAUDE.md

为仓库共享单个 `CLAUDE.md`。将其检入 git，并让整个团队每周多次贡献。每当 Claude 做错事情时，将其添加到 `CLAUDE.md`，以便 Claude 下次知道不要这样做。

<a href="https://x.com/bcherny/status/2007179840848597422"><img src="../assets/boris-3-jan-26/4.png" alt="共享 CLAUDE.md" width="50%" /></a>

---

## 5/ 在 PR 上标记 @claude 更新 CLAUDE.md

在代码审查期间，在你同事的 PR 上标记 `@claude` 将某些内容添加到 `CLAUDE.md` 作为 PR 的一部分。使用 Claude Code GitHub action ([install-@hub-action](https://github.com/apps/claude)) —— 这是 Boris 版本的复利工程。

<a href="https://x.com/bcherny/status/2007179842928947333"><img src="../assets/boris-3-jan-26/5.png" alt="在 PR 上标记 @claude" width="50%" /></a>

---

## 6/ 大多数会话从计划模式开始

大多数会话从计划模式开始（shift+tab 两次）。如果目标是写 Pull Request，使用计划模式并与 Claude 反复交流直到你喜欢它的计划。从那里，切换到自动接受编辑模式，Claude 通常可以一次完成。一个好的计划非常重要。

<a href="https://x.com/bcherny/status/2007179845336527000"><img src="../assets/boris-3-jan-26/6.png" alt="计划模式" width="50%" /></a>

---

## 7/ 使用斜杠命令进行内循环工作流程

对你每天做很多次的每个"内循环"工作流程使用斜杠命令。这节省了重复提示，并使 Claude 也能使用这些工作流程。命令检入 git 并位于 `.claude/commands/`。

示例：`/commit-push-pr` — 提交、推送并打开 PR。

<a href="https://x.com/bcherny/status/2007179847949500714"><img src="../assets/boris-3-jan-26/7.png" alt="斜杠命令" width="50%" /></a>

---

## 8/ 使用子代理自动化常见工作流程

定期使用几个子代理：`code-simplifier` 在 Claude 完成工作后简化代码，`verify-app` 有端到端测试 Claude Code 的详细说明，等等。将子代理视为自动化最常见工作流程 —— 类似于斜杠命令。

子代理位于 `.claude/agents/`。

<a href="https://x.com/bcherny/status/2007179850139000872"><img src="../assets/boris-3-jan-26/8.png" alt="子代理" width="50%" /></a>

---

## 9/ 使用 PostToolUse 钩子自动格式化代码

使用 `PostToolUse` 钩子格式化 Claude 的代码。Claude 通常开箱即用生成格式良好的代码，钩子处理最后 10% 以避免稍后在 CI 中出现格式错误。

```json
"PostToolUse": [
  {
    "matcher": "Write|Edit",
    "hooks": [
      {
        "type": "command",
        "command": "bun run format || true"
      }
    ]
  }
]
```

<a href="https://x.com/bcherny/status/2007179852047335529"><img src="../assets/boris-3-jan-26/9.png" alt="PostToolUse 格式化钩子" width="50%" /></a>

---

## 10/ 预允许权限而不是 --dangerously-skip-permissions

不要使用 `--dangerously-skip-permissions`。相反，使用 `/permissions` 预允许你知道在你的环境中安全的常见 bash 命令，以避免不必要的权限提示。大多数这些检入 `.claude/settings.json` 并与团队共享。

<a href="https://x.com/bcherny/status/2007179854077407667"><img src="../assets/boris-3-jan-26/10.png" alt="预允许权限" width="50%" /></a>

---

## 11/ 让 Claude 通过 MCP 使用你的所有工具

Claude Code 使用你的所有工具。它经常搜索和发布到 Slack（通过 MCP 服务器），运行 BigQuery 查询回答分析问题（使用 `bq` CLI），从 Sentry 获取错误日志等。Slack MCP 配置检入 `.mcp.json` 并与团队共享。

<a href="https://x.com/bcherny/status/2007179856266789204"><img src="../assets/boris-3-jan-26/11.png" alt="MCP 工具" width="50%" /></a>

---

## 12/ 用后台代理验证长时间运行的任务

对于非常长时间运行的任务，要么 (a) 提示 Claude 在完成后用后台代理验证其工作，(b) 使用代理 Stop 钩子更确定性这样做，或 (c) 使用 ralph-wiggum 插件（最初由 @GeoffreyHuntley 构思）。

<a href="https://x.com/bcherny/status/2007179858435281082"><img src="../assets/boris-3-jan-26/12.png" alt="长时间任务验证" width="50%" /></a>

---

## 13/ 给 Claude 一种验证其工作的方式

可能是从 Claude Code 获得良好结果最重要的事情 —— 给 Claude 一种验证其工作的方式。如果 Claude 有那个反馈循环，它会将最终结果的质量提高 2-3 倍。

Claude 测试 Boris 提交的每一个更改。

<a href="https://x.com/bcherny/status/2007179861115511237"><img src="../assets/boris-3-jan-26/13.png" alt="给 Claude 验证方式" width="50%" /></a>

---

## 来源

- [Boris Cherny (@bcherny) 在 X 上 — 2026 年 1 月 3 日](https://x.com/bcherny/status/2007179832300581177)