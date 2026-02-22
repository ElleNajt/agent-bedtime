# agent-bedtime

Claude Code hook that reminds you to go to bed. Every message you send past bedtime, Claude gets told to remind you to wrap up and go to sleep.

![screenshot](screenshot.png)

## How it works

A `UserPromptSubmit` hook that fires on every message you send. If the current hour is past bedtime, it injects a bedtime reminder into Claude's context via stdout (exit 0). Claude sees it and reminds you naturally.

## Install

Add to `~/.claude/settings.json`:

```json
{
  "hooks": {
    "UserPromptSubmit": [
      {
        "hooks": [
          {
            "type": "command",
            "command": "~/code/tools/agent-bedtime/bedtime-hook"
          }
        ]
      }
    ]
  }
}
```

## Config

Create `~/.config/agent-bedtime` (required):

```
BEDTIME=23:00
WAKEUP=06:00
```

Changes take effect immediately — the file is sourced on every hook invocation, so no restart needed.
