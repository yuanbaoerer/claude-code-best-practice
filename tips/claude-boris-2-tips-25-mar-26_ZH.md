# 压缩合并与 PR 大小分布 —— Boris Cherny 的技巧

Boris Cherny ([@bcherny](https://x.com/bcherny))，Claude Code 的创造者，于 2026 年 3 月 25 日分享的见解总结。

<table width="100%">
<tr>
<td><a href="../">← 返回 Claude Code 最佳实践</a></td>
<td align="right"><img src="../!/claude-jumping.svg" alt="Claude" width="60" /></td>
</tr>
</table>

---

## 1/ 单日 266 次贡献 —— 压缩合并

Boris 分享了他的 GitHub 贡献图，显示 **3 月 24 日有 266 次贡献** —— 来自 **141 个 PR，全部压缩合并**，每个 PR 中位数 **118 行**。

- 压缩合并将所有分支提交合并为目标分支上的单个提交 —— 保持历史清洁和线性
- 每个 PR = 一个提交使回滚整个功能变得容易并简化 `git bisect`
- 在高速 AI 辅助工作流程（141 PR/天）下，压缩是务实的选择 —— 分支内的各个"修复 lint"、"尝试这个"提交是噪音

<a href="https://x.com/bcherny/status/2038552880018538749"><img src="assets/boris-25-mar-26/1.png" alt="Boris Cherny — 266 次贡献，全部压缩合并" width="50%" /></a>

---

## 2/ PR 大小分布 —— 保持 PR 小

Boris 分享了那 141 个 PR 的大小分布，总共 **45,032 行更改**（添加 + 删除）：

| 指标 | 行数 (增+删) | 含义 |
|------|-------------:|------|
| **p50** | **118** | 中位数 PR 大小 — 一半的 PR 为 118 行或更少 |
| p90 | 498 | 90% 的 PR 低于 500 行 |
| **p99** | **2,978** | 只有约 1 个 PR 超过约 3K 行 |
| min | 2 | 最小 PR — 一个快速的 2 行修复 |
| max | 10,459 | 最大单个 PR — 可能是迁移或生成代码 |

- **中位数 118 行**意味着大多数 PR 是专注和可审查的，即使在 141 PR/天
- 分布严重右偏 —— 偶尔的大 PR 是不可避免的（批量重命名、迁移），但常态是紧凑
- 小 PR 降低合并冲突风险，更容易审查，并与压缩合并完美配合以便清洁回滚

<a href="https://x.com/bcherny/status/2038552880018538749"><img src="assets/boris-25-mar-26/2.png" alt="Boris Cherny — PR 大小分布表" width="50%" /></a>

---

## 来源

- [Boris Cherny (@bcherny) 在 X 上 — 2026 年 3 月 25 日](https://x.com/bcherny)