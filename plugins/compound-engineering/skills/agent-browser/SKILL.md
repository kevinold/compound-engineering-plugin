---
name: agent-browser
description: This skill should be used for browser automation - taking screenshots, navigating URLs, clicking elements, filling forms, checking console errors, or testing UI. Use instead of Playwright MCP or npx playwright. Provides the agent-browser CLI.
---

# agent-browser CLI

Browser automation CLI optimized for AI agents. This skill provides the commands and patterns for interacting with web pages programmatically.

## Pre-flight Check

Before using browser automation, verify agent-browser is installed:

```bash
command -v agent-browser >/dev/null 2>&1 || { echo "ERROR: Run 'npm install -g agent-browser && agent-browser install'"; exit 1; }
```

## Core Workflow

1. `agent-browser open <url>` - Navigate to URL
2. `agent-browser snapshot -i --json` - Get element refs (@e1, @e2, etc.)
3. `agent-browser click @e3` or `agent-browser fill @e4 "text"` - Interact
4. Re-snapshot if page state changed

## Commands

| Action | Command |
|--------|---------|
| Navigate | `agent-browser open <url>` |
| Get refs | `agent-browser snapshot -i --json` |
| Click | `agent-browser click @<ref>` |
| Fill | `agent-browser fill @<ref> "<text>"` |
| Type (slow) | `agent-browser type @<ref> "<text>" --slowly` |
| Screenshot | `agent-browser screenshot [path]` |
| Full page | `agent-browser screenshot --full [path]` |
| Wait | `agent-browser wait <ms>` or `agent-browser wait "<selector>"` |
| Console errors | `agent-browser errors` |
| Set viewport | `agent-browser set viewport <w> <h>` |
| Press key | `agent-browser press <key>` |
| Close | `agent-browser close` |

## Session Persistence

Use `--session <name>` to maintain browser state across commands:

```bash
export AGENT_BROWSER_SESSION=my-test
agent-browser open http://localhost:3000
agent-browser click @e5
agent-browser close
```

## Common Workflows

### Taking Screenshots

```bash
# Navigate and screenshot
agent-browser open http://localhost:3000
agent-browser screenshot page-screenshot.png

# Full page screenshot
agent-browser screenshot --full full-page.png

# Screenshot after interaction
agent-browser snapshot -i --json
agent-browser click @e5
agent-browser wait 1000
agent-browser screenshot after-click.png
```

### Form Interaction

```bash
agent-browser open http://localhost:3000/login
agent-browser snapshot -i --json
# Find the username field ref (e.g., @e3) and password field (e.g., @e4)
agent-browser fill @e3 "user@example.com"
agent-browser fill @e4 "password123"
agent-browser click @e5  # Submit button
agent-browser wait 2000
agent-browser snapshot -i --json
```

### Checking for Errors

```bash
agent-browser open http://localhost:3000
agent-browser errors
# Returns any console errors on the page
```

### Setting Viewport for Responsive Testing

```bash
# Desktop
agent-browser set viewport 1440 900

# Tablet
agent-browser set viewport 768 1024

# Mobile
agent-browser set viewport 375 812
```

## Troubleshooting

**agent-browser not found:**
```bash
npm install -g agent-browser
agent-browser install
```

**Session stuck:**
```bash
agent-browser close
```

**No refs in snapshot:**
The page may still be loading. Try:
```bash
agent-browser wait 2000
agent-browser snapshot -i --json
```

Run `agent-browser --help` for full reference.
