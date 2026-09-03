---
name: cleanup
description: Safely close a completed or paused task by verifying owned disposable state, preserving recoverable leftovers, and producing a truthful handoff. Use when a branch, worktree, slot, browser session, loop, or temporary artifact may need cleanup.
---

# Cleanup

Cleanup is part of the workflow, not an afterthought.

## Core Rule

Do not delete dirty, ambiguous, or unowned state just to make the workspace look
clean.

## Cleanup Ledger

Before removing anything, list the task-owned state:

- branch
- worktree
- slot or local app processes
- browser session or temporary auth state
- temporary data, screenshots, logs, or exports
- loops, watchers, or port-forwards

Mark each item as:

- `owned-and-clean`
- `owned-but-dirty`
- `unknown-owner`
- `already-gone`

Only `owned-and-clean` is eligible for automatic removal.

## Workflow

1. Record the task label and intended stop state.
2. Build the cleanup ledger.
3. Verify git state before removing a worktree or branch.
4. Stop slots, port-forwards, or browser state only when they are clearly owned.
5. Preserve evidence that is still useful for resume or audit.
6. Report every leftover plainly.

## Output Contract

```text
CLEANUP | task=<label> | status=<closed|paused|blocked>
REMOVED | <items or none>
LEFTOVERS | <dirty, unknown, shared, or intentionally preserved items>
PROOF | <git, process, browser, or data cleanup result>
RESUME | <none or exact next step>
```
