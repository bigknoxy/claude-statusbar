# Claude Code Status Line

Custom status line display for Claude Code showing model, path, git branch, context usage, and rate limits with live countdown timers.

## Features

- **Model** - Current Claude model in bold cyan
- **Path & Git** - Working directory and git branch (if available)
- **Context** - Usage percentage with color warnings (green <50%, yellow 50-75%, blinking red >75%)
- **Rate Limits** - 5-hour and 7-day windows with countdown timers
  - Displays remaining time in human-readable format (d/h/m)
  - Auto-updates every 10 seconds

## Setup

1. Copy `~/.claude/statusline-command.sh` to your system
2. Update `~/.claude/settings.json`:
   ```json
   "statusLine": {
     "type": "command",
     "command": "bash /Users/Joshua.Knox/.claude/statusline-command.sh",
     "refreshInterval": 10000
   }
   ```
3. Set environment variables:
   ```json
   "env": {
     "CLAUDE_CODE_DISABLE_1M_CONTEXT": "1",
     "CLAUDE_AUTOCOMPACT_PCT_OVERRIDE": "65"
   }
   ```

## Files

- `CLAUDE.md` - Project documentation and design notes
- `statusline-command.sh` - Main bash script (executable)

## Design

- Separators: `│` between major segments, `·` between rate limit entries
- Percentages right-aligned to 3 chars for alignment
- Countdown format: "1d 21h 25m" (days when ≥1 day), "45h 25m" (hours), "3m 42s" (minutes+seconds)
- Git lock-safe: uses `GIT_OPTIONAL_LOCKS=0`
