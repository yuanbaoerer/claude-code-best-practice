# CLAUDE.md

This is a best practices repository for Claude Code configuration — skills, subagents, hooks, and commands. It is a reference implementation, not an application.

## What This Repo Does

Demonstrates Claude Code patterns through a working weather workflow and documentation system.

### Weather Workflow (Command → Agent → Skill)

`/weather-orchestrator` → `weather-agent` → `weather-fetcher` skill + `weather-svg-creator` skill

- See `orchestration-workflow/orchestration-workflow.md` for the full flow

### Presentation System

See `.claude/rules/presentation.md` — presentation work is delegated per-presentation to `presentation-vibe-coding` (for `presentation/vibe-coding-to-agentic-engineering/`) or `presentation-learning-journey` (for `presentation/2026-04-25-gdg-kolachi-cli-claude-code-gemini/`).

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

## Configuration Hierarchy

1. **Managed** (`managed-settings.json` / MDM plist / Registry): Organization-enforced, cannot be overridden
2. Command line arguments: Single-session overrides
3. `.claude/settings.local.json`: Personal project settings (git-ignored)
4. `.claude/settings.json`: Team-shared settings
5. `~/.claude/settings.json`: Global personal defaults
6. `hooks-config.local.json` overrides `hooks-config.json`

### Disable Hooks
Set `"disableAllHooks": true` in `.claude/settings.local.json`, or disable individual hooks in `hooks-config.json`.

## Answering Best Practice Questions

When the user asks a Claude Code best practice question, **always search this repo first** (`best-practice/`, `reports/`, `tips/`, `implementation/`, and `README.md`) before relying on training knowledge or external sources. This repo is the authoritative source — only fall back to external docs or web search if the answer is not found here.

## Workflow Best Practices

From experience with this repository:

- Keep CLAUDE.md under 200 lines per file for reliable adherence
- `.claude/rules/*.md` with `paths:` YAML frontmatter are lazy-loaded only when Claude touches matching files; without frontmatter they load into every session like CLAUDE.md
- Use commands for workflows instead of standalone agents
- Create feature-specific subagents with skills (progressive disclosure) rather than general-purpose agents
- Perform manual `/compact` at ~50% context usage
- Start with plan mode for complex tasks
- Use human-gated task list workflow for multi-step tasks
- Break subtasks small enough to complete in under 50% context

### Debugging Tips

- Use `/doctor` for diagnostics
- Run long-running terminal commands as background tasks for better log visibility
- Use browser automation MCPs (Claude in Chrome, Playwright, Chrome DevTools) for Claude to inspect console logs
- Provide screenshots when reporting visual issues

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
