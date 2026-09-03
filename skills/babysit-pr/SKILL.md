---
name: babysit-pr
description: Run a quiet pull-request loop that checks for real changes, reports milestones, and stops at clear authority boundaries.
---

# Babysit PR

Use this as a recurring loop for a pull request or merge request.

## One tick only

Each run should:

1. read the saved cursor or last known state
2. read the current PR state
3. compare for changes
4. act only on pre-authorized steps
5. save the new state
6. exit

The scheduler owns recurrence. This skill does one pass.

## What to watch

Typical watch points:

- new commits
- new review comments
- CI state
- mergeability
- approvals
- deployment or runtime proof status

## Reporting

Notify only when something meaningful changed:

- a new blocker
- a request for human input
- green CI
- clean review state
- merge readiness

Stay quiet when nothing changed.

## Rules

1. Do not merge unless explicit authority is given.
2. Do not post review comments unless the loop was allowed to do that.
3. Keep the report short and tied to proof.
4. Save state after successful actions so the next tick is idempotent.

## Stop rules

Stop when:

- the PR is merged
- the PR is closed
- the PR needs a human decision
- the loop reaches its explicit end condition
