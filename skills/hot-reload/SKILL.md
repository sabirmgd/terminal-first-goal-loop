---
name: hot-reload
description: Drive debugging and feature completion through the smallest failing boundary first, reuse valid evidence, and keep the full journey as the last step. Use when repeated rebuilds, redeploys, or end-to-end reruns are slowing the task down.
---

# Hot Reload

This skill changes the order of proof, not the definition of done.

## The Loop

Repeat this loop until the boundary is green:

`exact failure -> smallest failing check -> smallest coherent fix -> same check again -> next boundary`

## Rules

1. Freeze the exact failing boundary before editing.
2. Pick the cheapest check that can prove the current claim.
3. Reuse accepted evidence unless the change actually invalidates it.
4. Move up one boundary at a time.
5. Keep the full browser, end-to-end, or release journey for last.
6. If the same defect class comes back twice, stop local patching and escalate to
   a deeper design or invariant review.

## Boundary Ladder

Typical order:

- static check or type check
- unit test
- integration or contract test
- hot-reload request
- slot or sandbox proof
- full browser or consumer journey
- release or deployed-runtime proof

## Evidence Reuse

When a change touches only one boundary, do not throw away unrelated green proof.
Only invalidate evidence that the change can actually affect.

A useful record after each step is:

- what failed
- what changed
- what proof stayed valid
- what proof must be rerun next

## Stop Conditions

Stop this loop and switch to a wider review when:

- the same root defect recurs twice
- the next fix would cross a larger architectural boundary
- the environment is the real blocker rather than the code
- the task now needs the final browser or release proof

## Output Contract

```text
HOT RELOAD | boundary=<summary> | level=<name>
FAILURE | <exact failing signal>
DELTA | <what changed>
REUSED | <still-valid proof>
NEXT | <smallest next check>
FULL JOURNEY | allowed|not-yet
```
