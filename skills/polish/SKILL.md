---
name: polish
description: "Run a fail-closed polish gate on the current branch diff: freeze scope, prove the baseline, simplify only changed files, run an independent review, verify fixes, and allow commit or push only after the reviewed candidate is still fresh and clean."
---

# Polish

This skill is the final cleanup and ship gate for a branch you already changed.
It is strict on purpose. The cost of a false green is higher than the cost of
one more review loop.

## What This Skill Does

- checks that the branch is a safe place to polish
- freezes the production scope to the current branch diff
- records baseline proof before changing anything
- simplifies only the changed files
- runs an independent review
- fixes only confirmed findings
- re-verifies the result
- allows commit or push only if the reviewed candidate is still the one being
  shipped

## Inputs

```text
$polish
$polish --review-only
$polish --no-commit
$polish --no-push
$polish --message "<commit subject>"
$polish --max-fix-rounds <n>
```

Rules:

- `--review-only`: read-only. Review and report only.
- `--no-commit`: finish the polish loop but do not commit or push.
- `--no-push`: commit if clean and allowed, but do not push.
- `--max-fix-rounds`: default `2`, hard cap `3`.

## Step 0: Preflight

Before doing anything else, verify:

- you are inside a git repo
- `HEAD` is on a branch, not detached
- the branch is not the default branch
- the branch has a real diff against its base
- the base branch is known

Collect and freeze:

- repo root
- branch name
- base branch or base commit
- changed production files
- current diff bytes
- current tree hash or diff hash

Why freeze it now: if the allowed production scope drifts mid-run, the review
you trusted is no longer reviewing the thing you ship.

## Scope Freeze

Production edits are limited to the original changed files.

Allowed outside that list:

- directly related regression tests
- proof helpers used only for verification

Not allowed outside that list:

- product code
- drive-by cleanup
- opportunistic refactors in untouched areas

If production scope expands, stop.

## Step 1: Baseline Proof

Before simplifying or fixing anything, run the smallest proof that matters for
this branch:

- relevant tests
- lint or typecheck
- build if needed
- focused runtime or browser proof when required

If the baseline is already broken and the reason is unclear, stop. Otherwise you
cannot tell whether polish made things better or worse.

## Step 2: Simplify Changed Files

Simplify only the frozen changed files.

Goals:

- remove obvious clutter
- reduce nesting
- improve naming
- delete dead code and debug leftovers
- keep behavior the same

If no meaningful simplification exists, that is fine. Say so and move on.

After simplification, rerun the relevant baseline proof. If it fails, stop.

## Step 3: Independent Review

Run a fresh independent review against the current frozen diff.

Requirements:

- the reviewer must be separate from the writer of the code
- the review must read the real diff, not a summary
- findings need stable ids so you can track them across rounds
- unconfirmed or malformed findings do not count as clean

If no independent review surface is available, stop. This skill is fail-closed.

## Finding Lifecycle

Track findings through these states:

- `open`
- `fixed`
- `rechecked`
- `closed`

Rules:

- fix only confirmed findings
- deduplicate by root cause
- if the same root cause survives two verified rounds, stop and escalate
- if a later round changes the diff, rerun review on the fresh candidate

## Step 4: Fix Only What The Review Proved

Work one round at a time. Do not keep patching forever.

For each round:

1. fix the confirmed findings
2. keep production edits inside the frozen file list
3. add or update regression tests only when needed to prove the fix
4. rerun the relevant proof
5. rerun the independent review on the fresh diff

If there is no progress, or the round cap is reached, stop.

## Step 5: Freshness Gate

Before commit or push, confirm the reviewed candidate is still fresh.

At minimum, recheck:

- repo root is unchanged
- branch is unchanged
- base is unchanged
- production file list is unchanged
- current diff hash matches the last reviewed diff hash

Why this matters: a commit, rebase, extra edit, or auto-format step can change
the candidate after review. If the hash changed, the review is stale.

If freshness fails, return to the review step. Do not ship stale review.

## Step 6: Commit And Push Authority

Commit and push are separate permissions.

Commit is allowed only when:

- the latest proof passed
- the latest independent review is clean
- confirmed findings are `0`
- the freshness gate passed
- the tree stayed inside the frozen production scope

Push is allowed only when:

- commit is allowed
- the task explicitly allows push
- there is something real to push
- the freshness gate still passes right before push

If commit or push authority is missing, stop with the verified tree and report
what remains.

## Stop Conditions

Stop with `clean` when:

- proof passes
- review is clean
- confirmed findings are `0`
- freshness passes
- requested commit or push action, if authorized, is complete

Stop with `blocked` when:

- preflight fails
- baseline proof fails unexpectedly
- production scope expands
- independent review is unavailable
- review output is malformed or stale
- the same root cause survives two verified rounds
- the fix-round cap is reached
- commit or push would exceed authority

## Output Contract

```text
POLISH | branch=<name> | state=<clean|blocked|review-only>
SCOPE | files=<count> | expanded=<yes|no>
BASELINE | result=<pass|fail> | proof=<short-summary>
REVIEW | findings=<count> | round=<n> | fresh=<yes|no>
VERIFY | result=<pass|fail> | evidence=<short-summary>
SHIP | commit=<done|skipped|blocked> | push=<done|skipped|blocked>
NEXT | <none or exact blocker>
```

## Final Rule

This skill is not "make the branch look nicer." It is "prove the branch is safe
to ship, or stop before it ships."
