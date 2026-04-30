# Claude Code Status Line Configuration

Custom status line display for Claude Code showing model, path/git branch, context usage, and rate limits.

## Files

- `~/.claude/settings.json` - Configuration file with environment variables and status line command
- `~/.claude/statusline-command.sh` - Bash script that formats and renders the status line

## Setup

Environment variables in `settings.json`:
- `CLAUDE_CODE_DISABLE_1M_CONTEXT=1` - Disable 1M context window
- `CLAUDE_AUTOCOMPACT_PCT_OVERRIDE=65` - Auto-compact at 65% context usage (vs default 80%)

Status line command points to the bash script with 10-second refresh interval:
```json
"statusLine": {
  "type": "command",
  "command": "bash /Users/Joshua.Knox/.claude/statusline-command.sh",
  "refreshInterval": 10000
}
```

`refreshInterval` (milliseconds) controls how often status line updates. Without it, status line only updates on events (tool finish, message sent), causing rate limit data to become stale. 10000ms (10 seconds) keeps data fresher.

## Status Line Segments

1. **Model** - Current model name (bold cyan)
2. **Path & Git** - Working directory (home shortened to ~) and git branch if available (magenta)
3. **Context** - Usage percentage with dynamic color coding:
   - Green: <50%
   - Yellow: 50-75%
   - Blinking red: >75%
   - Shows both percentage and raw tokens (e.g., "65% 130k/200k")
4. **Rate Limits** - 5-hour and 7-day windows with countdown timers to reset
   - Format: "5h" or "7d" label, percentage, countdown (e.g., "45h 53m" or "3m 42s")

## Design Notes

- Separators: `│` between major segments, `·` between rate limit entries
- Percentages right-aligned to 3 characters for consistent width
- Countdown timers use space separator between units for readability (e.g., "45h 53m" not "45h53m")
- Git branch detection uses `GIT_OPTIONAL_LOCKS=0` to avoid contention with other git operations
- All time calculations use current epoch time and are updated on each prompt
