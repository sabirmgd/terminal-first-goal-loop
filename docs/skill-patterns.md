# Reusable Skill Patterns

I use many project-specific skills, but most of them are versions of the same small set of ideas. This guide describes those ideas without tying them to one company or codebase.

## What a good skill contains

A useful skill answers these questions:

```text
When should it run?
What inputs does it need?
What may it read or change?
Which actions need human authority?
How does it prove success?
What state must it save?
When must it stop?
What does it report?
```

If a skill only contains a long prompt, it is advice. If it also defines access, proof, state, and stop rules, it can become a reliable workflow.

## The common patterns

### 1. Goal shaping

Turns a rough request into an outcome, scope, non-goals, proof, human decisions, and stop conditions.

Use it for long features, risky changes, and anything an agent should run without constant supervision.

### 2. Context gathering

Collects only the relevant ticket, document, design, email, transcript, code, and history for one task.

The skill should keep the source and date, distinguish intent from current behavior, and avoid dumping an entire mailbox or knowledge base into the prompt.

### 3. Access checking

Reports whether a system is available, partial, or unavailable. It identifies the account, project, tenant, cluster, credential authority, command surface, and a safe proof without printing secret values.

Keep access discovery read-only. Secret creation, rotation, and deployment belong in a separate skill.

### 4. Worktree and environment setup

Creates an isolated branch and worktree, assigns a runtime slot, starts only the needed services, runs a smoke check, and records the cleanup command.

This is what makes parallel mutating agents safe.

### 5. Feature delivery

Starts from the user journey, loads design context, reviews architecture and data, writes acceptance tests, builds the smallest useful slice, and proves it through the real consumer.

### 6. Bug fixing

Reproduces the report, finds the root cause, explains the choices, writes a failing test, fixes the shared cause, runs the same test, and verifies the shipped result.

The investigation part can be autonomous and read-only. The fix pauses only for a real product, security, data, or compatibility decision.

### 7. Cheapest-boundary verification

Starts with the smallest failing test, uses hot reload, advances one boundary at a time, and runs the full journey only after cheaper checks pass.

This pattern saves more time than another faster model when Docker builds or deployments dominate the loop.

### 8. Browser proof

Runs the real user journey, records the exact tested version, captures screenshots, checks the browser console and network, and cleans up the browser session.

Use a browser for user-facing proof. Do not substitute an API response for a UI claim.

### 9. Review and simplification

Reviews the exact diff, groups findings by root cause, fixes confirmed issues, simplifies only changed files, reruns affected tests, and asks a fresh verifier to check the final version.

Useful tools include Anthropic's [PR Review Toolkit](https://github.com/anthropics/claude-plugins-official/blob/main/plugins/pr-review-toolkit/README.md), [code-simplifier](https://github.com/anthropics/claude-plugins-official/blob/main/plugins/code-simplifier/agents/code-simplifier.md), and [Ponytail](https://github.com/DietrichGebert/ponytail).

### 10. Release and runtime proof

Ties source commit, CI run, built artifact, deployed runtime, health checks, logs, and user-visible behavior together.

A green pipeline does not prove the intended version is serving traffic. A healthy service does not prove the user journey works.

### 11. Notification and handoff

Sends milestone-based progress, proof, blockers, decisions, and the next action to chat or WhatsApp. It should never forward raw logs when a short explanation is enough.

### 12. Recurring loop

Runs one fresh tick, reads a saved cursor, compares current state, performs only allowed idempotent actions, saves state after success, stays quiet on no change, and stops at a clear boundary.

Examples include pull request babysitters, CI watchers, runtime monitors, knowledge capture, and cleanup.

## Sample skill: access checker

```markdown
---
name: access-check
description: Find and verify approved access without exposing secrets
---

## Inputs

- target system
- intended account, project, tenant, or cluster
- required operation

## Rules

1. Stay read-only.
2. Report secret locations and key names, never values.
3. Select the target explicitly. Do not trust global CLI defaults.
4. Return available, partial, or unavailable.

## Proof

Run the smallest safe command that proves the required capability.

## Stop

Stop before creating, rotating, copying, or deploying a secret.
```

## Sample skill: one loop tick

```markdown
---
name: loop-tick
description: Run one idempotent pass of a recurring workflow
---

1. Read the saved cursor.
2. Fetch current state.
3. Exit quietly if nothing changed.
4. Perform only pre-authorized actions.
5. Verify each action.
6. Save the new cursor after success.
7. Report at most five useful lines.
8. Stop. The scheduler owns recurrence.
```

The last line matters. A skill should do one pass. `/loop`, `/schedule`, cron, launchd, or another scheduler decides when the next pass starts.

## Sample skill: bug fix

```markdown
---
name: bugfix
description: Turn a bug report into a verified fix
---

1. Reproduce the claim.
2. Trace the root cause and affected callers.
3. Explain the possible fixes and recommend one.
4. Write a failing test in the permanent test framework.
5. Fix the shared cause with the smallest change.
6. Run the same test locally.
7. Review and simplify the exact diff.
8. Ship only with authority.
9. Run the same test in the affected environment.
10. Clean up the worktree, slot, browser, and test data.
```

## Keep skills portable

Store one main copy of a workflow. Claude Code, Codex, or another harness should use thin adapters that point to the same source instead of keeping several unrelated copies.

Avoid hardcoding a model version unless the workflow has measured evidence that it needs one. Describe the role instead: fast reader, strong builder, deep reasoner, independent reviewer, or visual designer.

Keep project facts outside generic workflow logic. A project adapter can supply repository paths, ticket prefixes, cloud accounts, test commands, and release rules.

## Common skill mistakes

- Hiding secrets or credentials inside the instructions.
- Treating a successful command as proof of the user outcome.
- Allowing a loop to act twice because it has no cursor.
- Mixing read-only discovery with production mutation.
- Letting a reviewer edit the same code it is judging.
- Copying the same workflow into several tools until the copies disagree.
- Running full Docker builds and end-to-end tests as the inner development loop.
- Reporting “done” while deployment, browser proof, cleanup, or a human decision is still open.
