---
name: pr-description
description: Write a pull request description with Overview, Files changed and Testing sections. Use when creating or updating a PR, or when asked to describe changes for review.
---

# Writing PR descriptions

Written for: the code reviewer, who has not seen the work and needs to check it.

## Title

Imperative mood, under about 70 characters, naming the main change. Good: "Add retry to input download". Avoid: "Updates" or "Changes to utils".

## Structure

Use these headings, in this order:

### Overview

Two to four sentences. What the change does and why. Name the user-visible effect (for example, "`npm run day -- 3 -j` now prints results as JSON"). Don't repeat the commit list.

### Files changed

Group by folder. For a folder with many files that changed the same way, give one bullet for the group. List files individually only when their logic changed. Say what changed, not how the code works. Mention deleted and renamed files explicitly.

### Testing

Numbered steps the reviewer can copy and run, each with the expected result. Include commands exactly as typed. Cover:

- The commands that prove the change works (for example, the test command should pass).
- Any checks that compare before and after behaviour, and how to run them.
- Anything that can't be checked automatically, such as a call to an external service, and say so.

If nothing was tested, say that plainly and say why. Don't invent steps.

### Known issues (optional)

Include this section only when there are problems the reviewer should know about before they start, such as a failing test that was already broken before the change. One bullet per issue, with the command that shows it.

## Example

```markdown
## Overview

Adds a `--json` flag to the runner so results can be piped into other tools. Plain output is unchanged when the flag is absent.

## Files changed

- `src/runner.js`: parses `--json` and prints each result as a JSON object.
- `src/utils/format.js` (new): formats results for JSON output.
- `README.md`: documents the flag.

## Testing

1. `npm test` passes.
2. `npm run day -- 1 -t` prints the same result lines as before.
3. `npm run day -- 1 -t --json` prints one valid JSON object per part. Check with `npm run day -- 1 -t --json | node -e "JSON.parse(require('fs').readFileSync(0))"`, which should exit with no error.
4. Not checked: running against real puzzle input from an external site. This needs a personal session token.

## Known issues

- `npm run day -- 6 -t` throws "Cannot read properties of undefined". This was broken before this change.
```

## Rules

- Keep the whole description to about 300 words, not counting the Testing commands.
- Write plain sentences. No filler such as "This PR introduces".
- Don't include personal inputs, session tokens, or `.env` contents.
- Don't claim something was tested unless it was run.
- When you create the PR yourself, end the description with this line, after a blank line. Don't add it to descriptions you write for the user to post:

  🤖 Generated with [Claude Code](https://claude.com/claude-code)
