# Configuration Hierarchy

Claude Code settings follow a priority order (highest to lowest):

1. **Managed** (`managed-settings.json` / MDM plist / Registry)
   - Organization-enforced, cannot be overridden

2. **Command line arguments**
   - Single-session overrides

3. **`.claude/settings.local.json`**
   - Personal project settings (git-ignored)

4. **`.claude/settings.json`**
   - Team-shared settings

5. **`~/.claude/settings.json`**
   - Global personal defaults

6. **Hook configs**
   - `hooks-config.local.json` overrides `hooks-config.json`

## Disable Hooks

Set `"disableAllHooks": true` in `.claude/settings.local.json`, or disable individual hooks in `hooks-config.json`.

## Hook System

Cross-platform sound notification system in `.claude/hooks/`:

- `scripts/hooks.py` — Main handler for Claude Code hook events
- `config/hooks-config.json` — Shared team configuration
- `config/hooks-config.local.json` — Personal overrides (git-ignored)
- `sounds/` — Audio files organized by hook event

### Hook Events

Configured in `.claude/settings.json`:

`PreToolUse`, `PostToolUse`, `UserPromptSubmit`, `Notification`, `Stop`, `SubagentStart`, `SubagentStop`, `PreCompact`, `SessionStart`, `SessionEnd`, `Setup`, `PermissionRequest`, `TeammateIdle`, `TaskCompleted`, `ConfigChange`

### Special Handling

Git commits trigger `pretooluse-git-committing` sound.
