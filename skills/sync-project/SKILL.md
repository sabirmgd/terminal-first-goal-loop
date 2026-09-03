---
name: sync-project
description: Fetch, prune, and report status across one repo or a multi-repo umbrella while preserving dirty trees and surfacing divergence instead of silently resetting anything.
---

# Sync Project

Use this when a project or umbrella workspace needs a clean read on branch state before planning, implementation, or reporting.

## Goal

Bring remote knowledge up to date without disturbing local work.

## Safe default actions

- fetch remotes
- prune stale remote references
- inspect branch state
- report divergence
- report dirty worktrees

This skill does not reset, discard, rebase, or rewrite local work by default.

## Workflow

For each repo in scope:

1. detect the current branch
2. check whether the tree is clean or dirty
3. fetch from the configured remote
4. prune stale remote references if safe
5. compare local branch, upstream branch, and merge base
6. report divergence and blocked states

## Output

Return one compact block per repo:

```text
Repo:
Current branch:
Dirty:
Upstream:
Ahead/behind:
Diverged:
Open blocker:
Next safe action:
```

## Rules

1. Never reset a dirty tree.
2. Never switch branches just to make the report look clean.
3. Never hide divergence.
4. If a repo has no upstream or no remote, say so plainly.
5. Preserve user worktrees and uncommitted work.

## Good use

- sync before cutting a worktree
- sync before planning a feature
- sync an umbrella project before a daily report

## Stop rules

Stop when:

- every repo in scope has a fresh remote view and status report
- a repo is blocked by auth or a dirty-tree condition that needs human judgment
