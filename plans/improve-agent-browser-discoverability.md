# Improve agent-browser Skill Discoverability

**Type:** Enhancement
**Created:** 2026-01-14
**Branch:** `kevin/use-agent-browser`

## Overview

Make the `agent-browser` skill more discoverable so AI agents automatically use it for browser automation tasks instead of attempting direct Playwright commands or hallucinating MCP tools.

## Problem Statement

An agent needed screenshots early in a task but used `npx playwright` directly instead of discovering the agent-browser skill. The root cause: the skill description doesn't contain enough trigger keywords that match how agents think about browser tasks.

## Solution

Update the skill description with action-oriented trigger phrases. One file, one line change.

**Current description:**
```yaml
description: This skill should be used when browser automation is needed for testing, screenshots, design iteration, or bug reproduction. It teaches how to use the agent-browser CLI for refs-based browser interaction.
```

**New description:**
```yaml
description: This skill should be used for browser automation - taking screenshots, navigating URLs, clicking elements, filling forms, checking console errors, or testing UI. Use instead of Playwright MCP or npx playwright. Provides the agent-browser CLI.
```

**Why this works:**
- Adds specific action verbs that match task language (taking, navigating, clicking, filling, checking)
- Explicitly mentions what NOT to use (Playwright MCP, npx playwright)
- Under 40 words (per Kieran's guidance)
- Third person as required ("This skill should be used")

## Implementation

### File to Modify

`plugins/compound-engineering/skills/agent-browser/SKILL.md` - Update line 3 (description in frontmatter)

### Steps

1. Edit the skill description
2. Update CHANGELOG.md
3. Test discovery with: "take a screenshot of localhost:3000"

## Acceptance Criteria

- [ ] Skill description updated with action-oriented keywords
- [ ] Description under 40 words
- [ ] Description stays single-line (no prettier wrapping)
- [ ] CHANGELOG.md entry added
- [ ] Manual test: asking "take a screenshot" triggers skill discovery

## What We're NOT Doing (Per Reviewer Feedback)

- ~~Create `/browser` wrapper command~~ - Unnecessary abstraction
- ~~Modify workflows:work~~ - Wrong solution for the problem
- ~~5-phase implementation~~ - Over-engineered for a 1-line change

If discovery is still broken after this change, we can iterate with additional measures.

## References

- Skill file: `plugins/compound-engineering/skills/agent-browser/SKILL.md:3`
- Reviewer feedback: DHH, Kieran, Simplicity all agreed on simplified approach
