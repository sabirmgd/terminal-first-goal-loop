---
name: babysit-prs
description: Run one quiet PR or MR babysitting tick that checks the real branch state, reviews only the new delta, and reports only meaningful changes. Use when a branch needs light monitoring for commits, comments, CI, mergeability, approvals, or human action without constant manual polling.
---

# Babysit PRs

This skill runs one tick, then stops. It is meant to be called again by
`/loop`, `/schedule`, cron, launchd, systemd, or another scheduler.

## Why This Exists

A branch can change while nobody is looking:

- new commits arrive
- CI flips red or green
- a reviewer leaves a comment
- mergeability changes
- the branch becomes ready for a human decision

The goal is to notice those changes without staring at the PR page and without
spamming the same update every few minutes.

## Inputs

The caller should provide or discover:

- PR or MR URL, or enough repo and branch context to find it
- local repo or worktree path if code inspection is needed
- cursor store path
- report route
- whether posting comments is allowed
- whether approval is allowed
- whether merge is allowed

If any write permission is not explicitly granted, treat it as `no`.

## One Tick

Each run should:

1. Load the saved cursor. If none exists, start with an empty baseline.
2. Fetch the current PR or MR state from the source of truth.
3. Compare it with the last cursor.
4. Inspect the actual code delta for new commits before trusting any reply or
   comment summary. Code wins over narration.
5. Report only meaningful changes.
6. Save the new cursor.
7. Exit.

## What To Watch

Track at least these fields:

- head commit SHA
- commit count or commit ids since last tick
- review comments and issue comments
- review decisions or approvals
- CI and required checks
- mergeability or conflict state
- open or closed or merged state

Use stable ids when the platform provides them. Do not key off free text alone.

## Delta-Only Review Rule

When there is a new head commit:

1. Compare the previous head SHA to the current head SHA.
2. Review only the new diff between those two points, not the whole branch from
   scratch.
3. If the previous head is unknown, fall back to the current branch diff
   against the base branch and record that this was a baseline tick.
4. If a human comment claims something was fixed, verify it in code before
   reporting it as resolved.

This keeps the loop cheap and stops it from rediscovering the same findings.

## Idempotency And Cursor

The cursor should remember enough to avoid duplicate updates, for example:

- last seen head SHA
- last seen comment ids
- last seen review ids
- last seen CI conclusion per check
- last seen mergeability state
- consecutive transient failure count

Only advance the cursor after the tick is understood. If the run half-fails, do
not move the pointer and pretend the event was handled.

## Quiet Path

If nothing meaningful changed, stay quiet.

Acceptable quiet output:

```text
BABYSIT PRS | state=quiet | target=<pr-or-mr>
```

Do not send "still watching" messages with no new proof.

## Reporting

Report only when one of these changes:

- new commit landed
- a new review comment or blocking thread appeared
- CI changed state
- mergeability changed
- the branch was approved, closed, or merged
- the loop hit a blocker or needs a human decision

Use proof-based status:

```text
BABYSIT PRS | target=<pr-or-mr> | state=<update|done|blocked>
HEAD | old=<sha-or-none> | new=<sha>
CI | changed=<checks-that-changed-or-none>
COMMENTS | new=<count-or-none> | unresolved=<count-or-none>
MERGE | state=<mergeable|conflicted|blocked|merged|closed>
NEXT | <next action or none>
```

## Authority Boundaries

- Reading PR, MR, CI, and code state is allowed.
- Posting a review comment is allowed only when the task explicitly says so.
- Approving is allowed only when the task explicitly says so.
- Merging is allowed only when the task explicitly says so.

If the loop reaches a point where a write would be useful but not authorized,
report that and stop. Do not silently act.

## Retry And Stop Rules

Retry only transient failures, such as timeouts or a temporary API error. Keep
a consecutive failure counter in the cursor.

Stop and report `blocked` when:

- the branch is merged
- the branch is closed
- the loop needs a human decision
- the same transient failure keeps repeating and reaches the retry cap
- repo access or platform access is unavailable

## Final Rule

This skill is a branch watcher, not a hidden reviewer or merger. It watches,
verifies the new delta, reports with proof, and stops at authority boundaries.
