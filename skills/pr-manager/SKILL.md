---
name: pr-manager
description: Manage the pull-request or merge-request lifecycle from exact repo and branch resolution through draft creation, updates, monitoring, and ready-for-review transitions, without merging or posting without authority.
---

# PR Manager

Use this when code changes need a pull request or merge request, or when an existing review should be monitored from the terminal.

## Default mode

Read-only by default.

Safe default actions:

- inspect an existing PR or MR
- resolve exact repo, head, and base
- list checks, comments, or merge state
- draft a PR body
- prepare an update or review comment preview

Creating, updating, commenting, requesting review, or merging needs explicit authority.

## Resolve exact identity first

Before any action, resolve:

```text
Hosting system
Repository
Head branch
Base branch
Current commit
Existing PR or MR number, if any
```

Do not guess the repo from the current shell if the task can name it explicitly.

## Draft workflow

Prepare a preview with:

```text
Action:
Repo:
Head:
Base:
Title:
Body:
Linked tickets:
Exact create or update command:
```

The preview should include:

- problem
- root cause or change summary
- proof
- known risks or follow-up

## Write workflow

Once authority is explicit:

1. resolve repo, head, base, and current commit again
2. create or update the PR or MR
3. read the saved result back
4. return the URL, state, and proof

## Monitor workflow

For a watch-style use, report:

```text
PR or MR:
Repo:
Head:
Base:
Checks:
Reviews:
Mergeability:
Next useful action:
```

## Rules

1. Never merge from this skill without explicit authority.
2. Never post comments or review requests without explicit authority.
3. Keep the repo/head/base exact. Those identities are part of the proof.
4. Do not call a branch ready just because local tests passed if required remote checks are still pending.

## Stop rules

Stop when:

- the read result is complete
- the draft preview is ready
- the authorized create or update is confirmed
- the PR or MR is waiting on human review or external checks
