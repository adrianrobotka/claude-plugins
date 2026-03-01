---
description: Configure jj workspace hooks for worktree isolation in the current project
allowed-tools: Bash(cat:*), Bash(jq:*), Read, Write
---

## Your Task

Configure the `WorktreeCreate` and `WorktreeRemove` hooks in the current project's `.claude/settings.local.json` so that `--worktree` uses jj workspaces instead of git worktrees.

**Why this is needed:** Claude Code doesn't pick up `WorktreeCreate`/`WorktreeRemove` hooks from plugins — they must be in project or user settings.

## Steps

1. **Read the current settings** at `.claude/settings.local.json` (it may not exist yet, or may have existing content to preserve).

2. **Merge the following hooks** into the settings JSON, preserving any existing keys:

```json
{
  "hooks": {
    "WorktreeCreate": [
      {
        "hooks": [
          {
            "type": "command",
            "command": "${CLAUDE_PLUGIN_ROOT}/scripts/jj-worktree-create.sh"
          }
        ]
      }
    ],
    "WorktreeRemove": [
      {
        "hooks": [
          {
            "type": "command",
            "command": "${CLAUDE_PLUGIN_ROOT}/scripts/jj-worktree-remove.sh"
          }
        ]
      }
    ]
  }
}
```

**IMPORTANT:** `${CLAUDE_PLUGIN_ROOT}` must be resolved to the actual absolute path of this plugin's installed location. To find it, look for the directory containing this command file — it will be something like `~/.claude/plugins/cache/muloka-claude-plugins/workspace-jj/<hash>/`. Use that resolved path in the settings.

3. **Write the updated settings** to `.claude/settings.local.json`.

4. **Confirm** by showing the user what was written and remind them to restart their Claude Code session for the hooks to take effect.
