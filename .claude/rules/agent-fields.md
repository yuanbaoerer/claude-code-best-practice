# Agent YAML Frontmatter Fields

Reference for the YAML frontmatter in `.claude/agents/<name>.md` files.

## All Fields

```yaml
---
name: agent-name               # Unique identifier (defaults to filename)
description: PROACTIVELY when... # Required — when to invoke this agent
model: sonnet                  # haiku, sonnet, opus, or inherit
tools: WebFetch, Read          # Tool allowlist (inherits all if omitted)
disallowedTools:               # Tools to deny
  - Bash
skills:                        # Skills preloaded into agent context
  - weather-fetcher
  - weather-svg-creator
maxTurns: 10                   # Max agentic turns before stopping
permissionMode: acceptEdits    # e.g., "acceptEdits", "plan", "bypassPermissions"
mcpServers:                    # MCP servers for this agent
  - my-mcp-server
hooks:                         # Lifecycle hooks (PreToolUse, PostToolUse, Stop...)
memory: project                # Persistent memory: user, project, or local
background: true               # Always run as background task
effort: high                   # low, medium, high, max
isolation: worktree            # Run in temporary git worktree
color: green                   # CLI output color
---
```

## Field Details

| Field | Type | Description |
|-------|------|-------------|
| `name` | string | Unique identifier. Defaults to filename |
| `description` | string | **Required.** Use `PROACTIVELY` for auto-invocation |
| `model` | string | `haiku`, `sonnet`, `opus`, or `inherit` |
| `tools` | list | Comma-separated tool allowlist. `Agent(agent_type)` syntax supported |
| `disallowedTools` | list | Tools removed from inherited or specified list |
| `skills` | list | Skill names preloaded at startup |
| `maxTurns` | int | Max turns before agent stops |
| `permissionMode` | string | `acceptEdits`, `plan`, `bypassPermissions` |
| `mcpServers` | list | MCP server names or inline configs |
| `hooks` | list | Hook events (PreToolUse, PostToolUse, Stop most common) |
| `memory` | string | `user`, `project`, or `local` |
| `background` | bool | Always run as background task |
| `effort` | string | `low`, `medium`, `high`, `max` |
| `isolation` | string | `worktree` = run in temporary git worktree |
| `color` | string | CLI color: `red`, `blue`, `green`, etc. |

## Key Pattern

Subagents **cannot** invoke other subagents via bash commands. Use the `Agent` tool:

```
Agent(subagent_type="agent-name", description="...", prompt="...", model="haiku")
```

`Task(...)` still works as an alias (renamed from Task in v2.1.63).
