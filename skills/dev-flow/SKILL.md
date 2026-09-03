---
name: dev-flow
description: Run local development with shared warm heavy infrastructure, per-worktree app slots, deterministic slot leases, hot versus passive modes, smoke checks, and safe stop or release behavior. Use when several worktrees or agents need fast local feedback without restarting the whole stack.
---

# Dev Flow

This skill exists to keep local AI development fast.

The expensive failure mode is obvious once you see it: every small edit triggers
another full environment boot, rebuild, or container restart. The agent spends
more time waiting than testing.

## Core Model

Split the local stack into two layers:

- shared heavy infrastructure that stays warm
- lightweight per-worktree app slots

A slot belongs to one worktree. Shared infrastructure belongs to the whole local
workspace.

## Required Capabilities

A usable dev-flow should support these operations somehow:

- `warm`: start or verify shared heavy infrastructure
- `hot`: attach the active worktree to native hot reload when available
- `passive`: start a background app slot for another worktree
- `list`: show active slots and leases
- `smoke`: prove a slot is actually usable
- `stop`: stop one slot only
- `release`: free the slot lease when the task is done
- `cold`: stop shared heavy infrastructure only when no active task still needs it

## Deterministic Leases And Ports

Each slot should get a deterministic lease and a deterministic port band.

The exact numbers are project-specific, but the pattern should be stable:

- one slot id maps to one fixed port band
- the same slot id keeps the same ports until released
- leases are stored outside the repo so multiple worktrees can see them
- the lease store is the authority, not guesswork from terminal history

## Modes

### Hot mode

Use hot mode for the task under active human attention.

This mode should prefer native hot reload where the repo supports it. It usually
runs in the foreground and should stop only the worktree's app processes when
interrupted.

### Passive mode

Use passive mode for background agent work, comparison testing, or another
parallel worktree. It should start the app slot and return without taking over
an interactive pane.

## Smoke

A slot is not ready just because a process started.

Run a small smoke check that proves the slot can answer the path that matters.
Good smoke checks include:

- one health request
- one login or auth bootstrap
- one tiny API request
- one browser page load when the work is UI-heavy

## Safety Rules

1. Shared heavy infrastructure must not be stopped by `stop` on a single slot.
2. `cold` is a separate action and should check for remaining active leases.
3. A broken slot lease should be repaired explicitly, not silently reused.
4. If a repo has no useful hot reload path, say that and fall back to the
   smallest honest passive loop.

## Output Contract

```text
DEV FLOW | worktree=<path-or-name> | slot=<id>
MODE | hot|passive
INFRA | warm|already-warm|blocked
SMOKE | pass|fail|blocked
STOP | <slot-only stop command or action>
RELEASE | <lease release command or action>
```
