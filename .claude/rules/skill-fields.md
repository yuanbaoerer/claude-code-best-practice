# SKILL.md YAML Frontmatter Fields

Reference for the YAML frontmatter in `.claude/skills/<name>/SKILL.md` files.

## All Fields

```yaml
---
name: skill-name              # Display name and /slash-command (defaults to directory name)
description: When this skill...  # Recommended — auto-discovery and autocomplete
argument-hint: [issue-number]    # Autocomplete hint shown in CLI
disable-model-invocation: true   # Prevent automatic invocation
user-invocable: false            # Hide from / menu — background knowledge only
allowed-tools:                   # Tools allowed without permission prompts
  - Read
  - Write
model: sonnet                   # Model: haiku, sonnet, or opus
context: fork                   # Run in isolated subagent context
agent: general-purpose          # Subagent type when context: fork
hooks:                          # Lifecycle hooks scoped to this skill
  - PreToolUse
---
```

## Field Details

| Field | Type | Description |
|-------|------|-------------|
| `name` | string | Display name for `/slash-command`. Defaults to directory name |
| `description` | string | When to invoke — shown in auto-complete and used for auto-discovery |
| `argument-hint` | string | Hint for slash command argument (e.g., `[issue-number]`) |
| `disable-model-invocation` | bool | Set `true` to prevent automatic model invocation |
| `user-invocable` | bool | `false` hides from `/` menu (agent skill only) |
| `allowed-tools` | list | Tools usable without permission prompts when skill is active |
| `model` | string | Override model: `haiku`, `sonnet`, `opus` |
| `context` | string | `fork` = run in isolated subagent context |
| `agent` | string | Subagent type for `context: fork` (default: `general-purpose`) |
| `hooks` | list | Hook events scoped to this skill |
