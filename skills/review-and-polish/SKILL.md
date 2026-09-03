---
name: review-and-polish
description: Run a role-separated finish pass with review, finding verification, simplification of changed files, and final proof.
---

# Review And Polish

Use this after implementation is functionally working and before calling it ready.

## Role split

Keep these roles separate:

- maker
- reviewer
- simplifier
- verifier

The maker can fix findings, but the reviewer and verifier should not be the same judgment pass.

## Workflow

1. Freeze the candidate or diff to review.
2. Run an independent review.
3. Confirm which findings are real.
4. Fix the confirmed findings in the smallest coherent batch.
5. Simplify only the changed files.
6. Re-run the affected proof.
7. Run a fresh verifier pass on the final candidate.

## Rules

1. Scope simplification to changed files.
2. Do not widen a bug-fix or feature branch with unrelated cleanup.
3. Keep the final report honest about what was source proof and what was runtime proof.
4. If a review claim is refuted, drop it instead of turning it into taste debt.

## Tooling

This is where tools such as a code review plugin, `code-simplifier`, and Ponytail fit:

- review for correctness and risk
- simplify for clarity
- use Ponytail earlier to avoid writing unnecessary code in the first place

## Stop rules

Stop when:

- confirmed findings are addressed
- the final verifier pass is clean
- the remaining issue is outside the branch scope
