---
name: watch-loop
description: Run one fresh idempotent watch tick against a task, queue, deployment, PR, issue tracker, inbox, or other changing source. Use when a loop should poll safely, save cursor state, stay quiet when nothing changed, and stop at a clear success or retry limit.
---

# Watch Loop

This skill runs one tick, not a daemon. That keeps each run fresh, resumable,
and easy to reason about. Scheduling belongs to `/loop`, `/schedule`, cron,
launchd, systemd, or another external scheduler.

## Contract

- One invocation performs one fresh check.
- The tick must be idempotent.
- The tick must save enough cursor state to avoid re-reporting old events.
- The quiet path is success: if nothing relevant changed, emit no noisy summary.
- Mutation is off by default. A watch tick may mutate only if the current task
  explicitly authorizes that exact action. Polling is cheap; accidental writes
  are not.

## Required Inputs

The caller must define:

- `subject`: what is being watched
- `cursor_store`: where last-seen state lives
- `authority`: read-only, or the exact allowed mutation
- `stop_condition`: what ends the loop
- `retry_cap`: how many consecutive retryable failures are allowed
- `report_route`: where a meaningful update should go

## Tick Workflow

1. Load the saved cursor. If none exists, create an empty baseline.
2. Discover current state from the source of truth, not from cached chat memory.
3. Compare current state with the cursor.
4. Classify the result:
   - `quiet`: nothing actionable changed
   - `update`: something changed and should be reported
   - `done`: the stop condition is satisfied
   - `blocked`: progress requires a human or unavailable dependency
   - `retryable`: a transient error occurred and the loop may try again later
5. Save the new cursor only after the current tick is fully understood. This
   avoids losing an event because a half-failed run moved the pointer.
6. Emit a compact update only for `update`, `done`, `blocked`, or `retryable`.

## Idempotence Rules

1. Never re-send the same event just because the loop ran again.
2. Use stable event keys when the source provides them. Otherwise store a
   conservative tuple such as state plus updated-at plus target identity.
3. If a mutation is authorized, it must be safe to retry or must record an
   explicit completion marker before another attempt.
4. Never infer progress from a stale local note when the remote source can be
   checked directly.

## Retry Rules

1. Retry only transient failures: timeout, rate limit, temporary auth cache, or
   known flaky upstream response.
2. Keep a consecutive failure counter in the cursor store.
3. When the counter reaches `retry_cap`, stop returning `retryable` and return
   `blocked` with the exact reason.
4. Do not hide repeated failure behind silence. If the loop keeps failing, that
   is the update.

## Quiet Path

When nothing meaningful changed:

```text
WATCH LOOP | subject=<name> | state=quiet
```

Do not send raw logs, stack traces, or repeated "still working" messages. The
point is signal, not chatter.

## Update Contract

```text
WATCH LOOP | subject=<name> | state=<update|done|blocked|retryable>
CURSOR | before=<summary> | after=<summary>
PROOF | check=<command-or-api> | result=<short-result>
NEXT | action=<next-step-or-none>
```

## Good Uses

- poll a PR until checks finish
- watch a deployment until the expected revision is live
- check a queue or inbox for a matching event
- babysit a long-running agent until it needs a decision
- monitor a ticket, review thread, or approval gate

## Bad Uses

- long-lived background loops embedded inside one prompt
- mutation without explicit authority
- progress spam every tick
- raw log forwarding as the primary output
