# 计划任务实现

![最后更新](https://img.shields.io/badge/最后更新-Mar_10%2C_2026-white?style=flat&labelColor=555)

<table width="100%">
<tr>
<td><a href="../">← 返回 Claude Code 最佳实践</a></td>
<td align="right"><img src="../!/claude-jumping.svg" alt="Claude" width="60" /></td>
</tr>
</table>

---

<a href="#loop-demo"><img src="../!/tags/implemented-hd.svg" alt="已实现"></a>

`/loop` 技能用于按 cron 间隔调度周期性任务。下面是 `/loop 1m "tell current time"` 的演示 — 一个每分钟触发的简单周期性任务。

---

## Loop 演示

### 1. 调度任务

<p align="center">
  <img src="assets/impl-loop-1.png" alt="/loop 1m tell current time — 调度和 cron 设置" width="100%">
</p>

`/loop 1m "tell current time"` 解析间隔（`1m` → 每 1 分钟），创建 cron 任务并确认调度。关键说明：

- Cron 的最小粒度是 **1 分钟** — `1m` 映射到 `*/1 * * * *`
- 周期性任务 **3 天后自动过期**
- 任务是 **会话范围** — 仅存在内存中，Claude 退出时停止
- 随时可用 `cron cancel <job-id>` 取消

---

### 2. Loop 运行中

<p align="center">
  <img src="assets/impl-loop-2.png" alt="每分钟触发的周期性任务" width="100%">
</p>

任务每分钟触发，运行 `date` 并报告当前时间。每次迭代触发异步 **UserPromptSubmit** 和 **Stop** 钩子 — 与本仓库声音通知系统使用的相同钩子系统。

---

## ![如何使用](../!/tags/how-to-use.svg)

```bash
$ claude
> /loop 1m "tell current time"
> /loop 5m /simplify
> /loop 10m "check deploy status"
```

---

## ![如何实现](../!/tags/how-to-implement.svg)

`/loop` 是内置 Claude Code 技能 — 无需设置。它在底层使用 cron 工具（`CronCreate`、`CronList`、`CronDelete`）管理周期性调度。