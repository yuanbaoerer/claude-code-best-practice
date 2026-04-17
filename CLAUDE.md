# CLAUDE.md

This is a best practices repository for Claude Code configuration — skills, subagents, hooks, and commands. It is a reference implementation, not an application.

## What This Repo Does

Demonstrates Claude Code patterns through a working weather workflow and documentation system.

### Weather Workflow (Command → Agent → Skill)

`/weather-orchestrator` → `weather-agent` → `weather-fetcher` skill + `weather-svg-creator` skill

- See `orchestration-workflow/orchestration-workflow.md` for the full flow

### Presentation System

All presentation edits go to the `presentation-curator` agent — never edit `presentation/index.html` directly.

## Key Patterns

- **Commands** for workflows (`.claude/commands/*.md`)
- **Agents** for specialist roles with preloaded skills (`.claude/agents/*.md`)
- **Skills** for reusable task instructions (`.claude/skills/*/SKILL.md`)
- **Rules** for conditional, file-pattern-scoped guidance (`.claude/rules/*.md`)
- **Hooks** for event-driven automation (`.claude/hooks/`, `hooks-config.json`)

## How to Answer Questions

When asked a Claude Code best practice question, **search this repo first**:
`best-practice/`, `reports/`, `tips/`, `implementation/`, `README.md`

Only fall back to external docs or web search if not found here.

## Workflow Rules

- Keep this file under 200 lines — move detailed specs to `.claude/rules/`
- Use `/compact` proactively at ~50% context usage
- Use plan mode for complex tasks
- Break large tasks so each piece fits in under 50% context
- For multi-step tasks, use a human-gated task list

## Git Commit Rules

**One topic per commit.** Create separate commits per file — never bundle unrelated changes.

Example: if `README.md`, `best-practice/claude-subagents.md`, and a skill file all change:

1. `git add README.md` → commit with README-specific message
2. `git add best-practice/claude-subagents.md` → commit with subagents-specific message
3. `git add .claude/skills/weather-fetcher/SKILL.md` → commit with skill-specific message

## Debugging

- Run `/doctor` for diagnostics
- Run long terminal commands as background tasks (better log visibility)
- Use browser automation MCPs to inspect console logs
- Provide screenshots when reporting visual issues

## Detailed Specs (Rules)

For technical field references, see these rules:

- `.claude/rules/markdown-docs.md` — Documentation standards
- `.claude/rules/presentation.md` — Presentation system rules
- `.claude/rules/skill-fields.md` — SKILL.md YAML frontmatter fields
- `.claude/rules/agent-fields.md` — Agent YAML frontmatter fields
- `.claude/rules/config-hierarchy.md` — Settings/configuration hierarchy
