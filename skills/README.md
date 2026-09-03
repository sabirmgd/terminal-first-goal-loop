# Skills Catalog

These skills are the reusable core of the terminal-first workflow.

They are written to be portable:

- no project names
- no personal paths
- no secrets
- no hardcoded notification targets

Private adapters should provide the real repos, ticket systems, ports, credentials, and transport routes.

## Shape the Work

### `start-work`

Choose the lane, define the first safe action, and stop the task from starting as a vague blob.

### `prepare-context`

Collect only the context that helps the task move, and keep source and freshness visible.

### `shape-goal`

Turn a rough request into a bounded long-running goal with proof, milestones, and stop rules.

## Isolate and Speed Up the Work

### `worktree`

Protect dirty or shared checkouts and give each mutable task its own branch and folder.

### `dev-flow`

Keep heavy infrastructure warm and attach each worktree to a deterministic slot.

### `hot-reload`

Climb the proof ladder from the smallest failing check instead of repaying full rebuilds.

## Move the Work

### `deliver-feature`

Run a feature from user journey to proof without letting the browser or full rebuild become the default inner loop.

### `reproduce-bug`

Turn a bug claim into one same-run proof and stop before mutation.

### `review-and-polish`

Freeze the scope, review the exact candidate, simplify the changed files, verify again, and fail closed if the proof does not hold.

## Control the Edges

### `access-check`

Prove whether the required access path is available without leaking the secret that grants it.

### `watch-loop`

Run one fresh, idempotent watch tick with a saved cursor and a quiet path.

### `progress-update`

Send a milestone-based update with proof, next step, and blocker information.

### `release-proof`

Bind source, CI, artifact, runtime, and consumer proof to the same candidate.

### `cleanup`

Clean up only what is clearly owned and disposable, and leave a safe handoff for everything else.

## Install

List the package:

```bash
npx skills add sabirmgd/terminal-first-goal-loop --list
```

Install the full set:

```bash
npx skills add sabirmgd/terminal-first-goal-loop --skill '*'
```

Install a single skill:

```bash
npx skills add sabirmgd/terminal-first-goal-loop --skill shape-goal
```

## Notes

- These skills are intentionally generic.
- Community and native capabilities are listed in [docs/ecosystem.md](../docs/ecosystem.md).
- Public-safe does not mean toothless. Each skill still defines authority, proof, and stop conditions.
