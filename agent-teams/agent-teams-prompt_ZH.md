创建一个代理团队来构建时间编排工作流程，以视觉 SVG 卡片形式显示当前迪拜时间。该工作流程遵循
**命令 → 代理 → 技能** 架构模式：

- 命令编排流程并处理用户交互
- 代理使用预加载技能获取迪拜的实时时间
- 技能根据获取的数据创建视觉 SVG 时间卡片

**重要**：所有文件必须创建在 `agent-teams/.claude/` 内 —— **不要**放在仓库根目录的 `.claude/` 下。这
使代理团队的输出自包含且可通过 `cd agent-teams && claude` 运行。
**不要**引用或复制现有的天气工作流程 —— 从头开始构建所有内容。

分配这些队友：

1. **命令架构师** — 设计和实现 `/time-orchestrator` 命令，位于
   `agent-teams/.claude/commands/time-orchestrator.md`。命令应该：
   - 通过 Agent 工具（**不是** bash）调用 time-agent 获取迪拜、阿联酋的当前时间（Asia/Dubai 时区，UTC+4）
   - 通过 Skill 工具调用 time-svg-creator 技能，根据获取的时间数据渲染 SVG 卡片
   - 在 frontmatter 中使用 model: haiku
   - 包含关键要求：顺序流程、正确的工具使用（Agent 工具用于代理，Skill 工具用于技能）以及输出摘要
   通过共享任务列表与其他队友协调，就组件之间传递的数据契约（{time, timezone, formatted}）达成一致。

2. **代理工程师** — 设计和实现 `time-agent`，位于
   `agent-teams/.claude/agents/time-agent.md` 及其预加载的 `time-fetcher` 技能，位于
   `agent-teams/.claude/skills/time-fetcher/SKILL.md`。代理应该：
   - 使用 Bash 获取迪拜时间（Asia/Dubai，UTC+4），命令为 `TZ='Asia/Dubai' date '+%Y-%m-%d %H:%M:%S %Z'`
   - 将时间值、时区名称和格式化字符串返回给命令
   - 使用 frontmatter：tools (Bash)、model: haiku、color: blue、maxTurns: 3
   - 通过 `skills:` 字段预加载 time-fetcher 技能
   time-fetcher 技能（`agent-teams/.claude/skills/time-fetcher/SKILL.md`）应包含获取迪拜时间的 bash 命令、
   预期输出格式，并设置 user-invocable: false，因为它是仅代理的领域知识。
   将商定的数据契约发布到共享任务列表，以便命令架构师和技能设计师对齐接口。

3. **技能设计师** — 设计和实现 `time-svg-creator` 技能，位于
   `agent-teams/.claude/skills/time-svg-creator/SKILL.md`，以及支持文件 `reference.md`（SVG 模板 + 输出模板）
   和 `examples.md`（示例输入/输出对）。技能应该：
   - 从调用上下文接收时间值、时区和格式化字符串
   - 为迪拜创建自包含的 SVG 时间卡片，显示当前时间
   - 将 SVG 写入 `agent-teams/output/dubai-time.svg`
   - 将 markdown 摘要写入 `agent-teams/output/output.md`
   - 使用提供的确切时间 —— 绝不重新获取
   - 将模板保存在 reference.md（带占位符的 SVG 标记、markdown 输出模板），示例对保存在 examples.md
   同时创建 `agent-teams/output/` 目录用于输出文件。

所有三个队友应在共享任务列表中创建任务来协调数据契约：代理返回 {time, timezone, formatted}，
命令通过上下文传递它，技能消费它。
由于组件是独立的，所有三个并行启动 —— 它们只需要就数据接口达成一致，
不需要等待彼此的实现。