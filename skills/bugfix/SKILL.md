---
name: bugfix
description: Run a real bug-fix workflow from repro to shipped proof. Use when the user wants the full fix, or after investigation already proved the bug. The workflow includes reproduction, decision-ready investigation, durable failing tests, isolated worktree, shared-cause fix, hot-reload or slot proof, browser proof when relevant, review, simplification, shipped proof, cleanup, and handoff.
---

# Bugfix

This skill is the full bug-fix lane.

It is allowed to do real work, but only after the bug is proven and the expected behavior is clear enough to implement safely.

## Two valid modes

Investigation-only mode:

- reproduce the claim
- trace the root cause
- explain realistic fix options
- recommend one option
- stop before code changes

Full fix mode:

- use when the user asked to fix it, approved the investigation, or already supplied valid repro evidence
- run the full loop through proof and handoff

## Gates you must enforce

- never fix a bug that was not reproduced unless the user explicitly supplied current approved repro evidence
- never commit on `main`
- never let two mutating agents edit the same checkout
- never call local green "done" when shipped proof is still required
- never merge, deploy, or post external updates without explicit authority

## Step 0: scope the claim

Extract:

- the exact bug claim
- the intended behavior
- the affected surface
- any role, tenant, or environment assumption
- whether the fix is internal-only or user-visible

If the claim is user-visible, plan for two proof surfaces: one fast implementation proof and one consumer-facing proof.

If the report is ambiguous, the first job is to narrow the expectation, not to start coding.

## Step 1: reproduce it

Use `bug-repro` unless the user already supplied current valid repro evidence.

Interpret the result strictly:

- `yes`: continue
- `no`: report not reproduced and stop
- `inconclusive`: report blocker and stop unless the user explicitly wants environment repair next

Carry the repro bundle and expected contract forward. The fix should prove the same claim that the repro proved.

## Step 2: investigate before touching code

Trace the shared cause, not only the first failing line.

Read:

- the local guidance for the target repo
- the callers and callees around the failure
- the nearest existing tests and proof commands
- the runtime shape if the bug crosses process, queue, or browser boundaries

Produce a decision-ready brief even in full-fix mode:

```text
Claim
Reproduced?
Root cause
Affected surfaces
Fix options
Recommended option
What "go" changes
```

If there are several valid fixes with different product behavior, data shape, or risk, stop here and hand the decision back.

## Step 3: isolate the work

Before editing:

1. inspect `git status`
2. inspect `git worktree list`
3. fetch the latest base
4. create a clean worktree and branch from that base

If the main checkout is dirty, preserve it. Use another clean checkout or worktree as the base owner. Do not "clean up" another task just to start this one.

If the bug needs both product changes and a durable cross-surface regression, keep those changes clearly separated when the repos or test homes are separate.

## Step 4: write the failing durable test

The bug is not fixed until its proof has a home.

Prefer, in order:

- the existing integration or contract suite
- the existing API or end-to-end suite
- the nearest permanent test framework already used by the repo
- a focused local test only when no stronger permanent home exists

Rules:

1. write the failing test before the real fix
2. keep the expected behavior strict
3. do not weaken the assertion to match the bug
4. if the bug is user-visible, add or preserve a user-visible proof path, not only an internal unit test

If the repository genuinely has no durable test home, say that explicitly and leave the smallest repeatable proof you can.

## Step 5: fix the shared cause

Fix once at the shared boundary when possible.

Use the hot-reload ladder during implementation:

```text
exact failure -> smallest failing check -> minimal fix -> same check green -> next boundary
```

Guidelines:

- prefer reuse over new abstraction
- prefer deletion over more code
- keep the diff scoped to the bug
- if one fix changes behavior for several callers, prove those callers too

Do not use the full end-to-end path as the inner debugging loop if a smaller boundary can expose the same failure.

## Step 6: prove it locally and in the right runtime

Pick the cheapest honest proof first:

- unit, component, or focused local test
- integration or API proof
- hot-reload or app slot proof
- browser journey for user-facing behavior

If the stack supports warm shared infrastructure and isolated runtime slots, keep the expensive shared services warm and use a task-specific slot for the changing code.

Required proof by surface:

- backend or API bug: durable test plus direct API or integration proof
- browser bug: durable test where possible plus browser proof with screenshot or visible evidence
- cross-surface bug: prove both the internal contract and the visible result

Do not mistake a starting container, green compilation, or healthy process for proof that the bug is fixed.

## Step 7: review the exact change

Run an independent review on the exact candidate.

The review should answer:

- is the fix correct?
- did it create a sibling regression?
- is the bug really fixed at the shared cause?
- did the test actually lock the behavior?

Keep taste-only comments out of the critical path unless the user explicitly asked for that level of polish.

## Step 8: simplify the changed files

After correctness is in place, simplify only the files changed for this bug.

The goal is to remove unnecessary branching, duplication, or accidental complexity without widening scope.

Re-run the affected proof after simplification. A simplifier pass that changes behavior failed.

## Step 9: prove the shipped or affected environment

When the bug matters outside local development, run the same contract against the affected environment or candidate runtime.

Examples:

- preview or sandbox
- staging
- exact deployed slot
- production, only when that proof is safe and expected

This is where many false greens die. Local green is not shipped green.

If shipped proof is required but unavailable, report the exact blocker. Do not silently downgrade the definition of done.

## Step 10: cleanup and handoff

Clean only task-owned disposable state:

- worktree
- app slot or local process
- temporary browser state
- scratch evidence that no longer needs to live

Do not remove:

- dirty or ambiguous worktrees
- shared warm infrastructure you do not own
- reusable evidence that the next reviewer still needs

Return one compact handoff:

```text
BUGFIX | status=<investigated|fixed|blocked>
CLAIM | <plain-language bug>
ROOT CAUSE | <what was actually wrong>
FIX | <what changed>
PROOF | <tests, slot, browser, and shipped proof>
REVIEW | <clean or key resolved findings>
SIMPLIFY | <what was simplified or none>
LEFTOVERS | <none or exact blockers, waits, or follow-ups>
NEXT | <none or exact next human action>
```

## Stop conditions

Stop immediately when:

- the bug was not reproduced
- the repro is inconclusive and the blocker is outside scope
- the intended behavior is genuinely unclear
- the next step requires human authority for merge, deploy, release, or destructive action

In investigation-only mode, stop after the decision-ready brief.

In full-fix mode, stop only when the durable proof is green, the review and simplification passes are complete, and any missing shipped proof is stated plainly instead of hidden.
