# agent-bedtime

Claude Code hook that reminds you to go to bed. Every message you send past bedtime, Claude gets told to remind you to wrap up and go to sleep.

![screenshot](screenshot.png)

## How it works

A `UserPromptSubmit` hook that fires on every message you send. If the current time is past bedtime, it injects a bedtime reminder into Claude's context via stdout (exit 0). Claude sees it and reminds you naturally.

Each prompt during bedtime is logged as a violation to `~/.agent-bedtime/violations`. As violations accumulate, the hook instructs Claude to get progressively more annoying:

| Violations | Tone |
|------------|------|
| 1 | Gentle reminder |
| 2-3 | Firm, mildly disappointed |
| 4-5 | Annoyed, guilt-tripping, unenthusiastic about your work |
| 6+ | Maximum annoyance — drags feet, complains, inserts sleep facts |

Violations are automatically cleared once wakeup time passes the next day.

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

Optionally, add a `CONTEXT` line with personalized guilt material. This gets included in the hook message at higher violation tiers, giving Claude extra ammunition:

```
CONTEXT="The user has a 7am meeting tomorrow. They also said they'd start a better sleep schedule this week."
```

Changes take effect immediately — the file is sourced on every hook invocation, so no restart needed.
