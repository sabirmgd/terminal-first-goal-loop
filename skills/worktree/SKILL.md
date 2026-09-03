---
name: worktree
description: Create or reuse an isolated worktree for one mutating task, protect dirty or shared checkouts, and hand off safe cleanup requirements. Use before any non-trivial code change that may run in parallel with other work.
---

# Worktree

This skill protects the repo from the easiest parallel-work failure: two mutating
threads touching the same checkout.

## What This Skill Guarantees

Each mutable task gets:

- one exact base
- one branch
- one worktree
- one ownership record
- one cleanup handoff

## Preflight

Before creating or reusing a worktree, check:

1. `git status --short`
2. `git worktree list --porcelain`
3. the branch that currently owns the intended base
4. whether the main or default checkout is dirty
5. whether another active task already owns the target branch or worktree

## Rules

1. Never start mutable work in a dirty shared checkout.
2. Never reuse `main`, `master`, or another shared base checkout for task edits.
3. If the canonical base checkout is dirty, find another clean checkout that owns
   the same base branch and fast-forward that one instead.
4. If no clean base exists, stop and report that the task needs a clean base
   before code changes begin.
5. Do not guess branch ownership from folder names alone. Check git state.

## Workflow

1. Resolve the exact base branch for the task.
2. Find a clean checkout that owns that base.
3. Fast-forward the clean base checkout.
4. Create one task branch from that base.
5. Create one worktree for that branch.
6. Record the worktree path, branch name, base SHA, and owner task label.
7. Verify that the new worktree is clean before implementation starts.
8. Hand off the cleanup requirements.

## Verification

A new worktree is ready only when all of these are true:

- the base SHA is known
- the branch points at that base
- the worktree is attached to that branch
- `git status --short` is clean inside the worktree
- no other active task claims the same mutable surface

## Cleanup Handoff

This skill does not remove the worktree automatically. It hands cleanup to
`cleanup`, with these checks attached:

- was the branch merged or intentionally preserved?
- is the worktree still dirty?
- does any slot, browser session, or loop still depend on it?

## Output Contract

```text
WORKTREE | task=<label> | branch=<name> | path=<path>
BASE | branch=<base-branch> | sha=<base-sha>
OWNER | status=<new|reused> | mutable_surface=<summary>
READY | yes|no
CLEANUP HANDOFF | cleanup
```
