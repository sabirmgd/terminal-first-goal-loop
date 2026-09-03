---
name: polish-change
description: Freeze the scope of a finished change, review the exact candidate, simplify only what changed, verify again, and fail closed if the proof breaks. Use when implementation is mostly done and the job becomes cleanup, review, and signoff quality.
---

# Polish Change

This skill is the ship gate for a local change, not the feature builder.

## Core Rule

The maker should not be the only judge of the finished work.

## Workflow

1. Freeze the scope to the current intended change.
2. Record the proof that already passes.
3. Run an independent review of the exact candidate.
4. Fix confirmed issues in the smallest coherent batch.
5. Simplify only the changed files unless scope is intentionally expanded.
6. Run the affected proof again.
7. Use a fresh verifier pass before calling the change clean.

## Review Model

Use the strongest available review stack in the active environment.

Typical ingredients:

- a code review pass
- a simplifier pass
- a fresh verifier pass

When official review or simplifier capabilities exist, prefer them over re-inventing the same workflow locally.

## Rules

- do not quietly expand production scope during polish
- do not count an old green run as proof for a new candidate
- do not treat style-only suggestions as blockers when behavior and required quality gates are already clean
- do not merge or deploy just because polish passed

## Output Contract

```text
POLISH | scope=<short description> | candidate=<id-or-branch>
REVIEW | <confirmed findings or clean>
SIMPLIFY | <what changed or none>
VERIFY | <latest proof result>
STATUS | <clean|blocked|needs-fix>
NEXT | <none or exact remaining action>
```

## Stop Conditions

- stop when review and proof are both clean
- stop when the same class of issue keeps returning and needs a deeper redesign
- stop immediately if the frozen scope expands without explicit intent
