# Skills Catalog

The skills use concrete names because they should feel like tools, not chapters in a methodology book.

`workflow` is the front door. The other skills do one recognizable job and can also be used directly.

## Start here

| Skill | What it does |
| --- | --- |
| [`workflow`](workflow/SKILL.md) | Turns a message, ticket, screenshot, voice dump, review request, or bug report into the right lane, context, proof, isolation, and next action |

## Context and work systems

| Skill | What it does |
| --- | --- |
| [`context`](context/SKILL.md) | Builds a small sourced context pack from code, tickets, docs, design, email, calls, and runtime evidence |
| [`jira-manager`](jira-manager/SKILL.md) | Reads, searches, drafts, creates, updates, and links Jira tickets with write previews |
| [`docs-manager`](docs-manager/SKILL.md) | Reads, drafts, versions, and publishes Confluence or wiki pages with source grounding |
| [`access-check`](access-check/SKILL.md) | Proves which account, target, and access path work without exposing secret values |

`context` can call `jira-manager` or `docs-manager`. It can also use configured Figma, Gmail, Fireflies, or other MCPs directly. Those account-specific connectors stay outside the public skill pack.

## Code, debugging, and proof

| Skill | What it does |
| --- | --- |
| [`bug-repro`](bug-repro/SKILL.md) | Produces one standalone same-run repro with a strict yes, no, or inconclusive result, then stops |
| [`bugfix`](bugfix/SKILL.md) | Runs the full bug lane from repro and decision-ready investigation to failing test, shared-cause fix, review, and shipped proof |
| [`worktree`](worktree/SKILL.md) | Creates or reuses one isolated branch and worktree without disturbing dirty shared checkouts |
| [`dev-flow`](dev-flow/SKILL.md) | Keeps heavy infrastructure warm and gives each worktree a deterministic app slot |
| [`hot-reload`](hot-reload/SKILL.md) | Fixes the smallest failing boundary first and saves the full journey for last |
| [`browser-proof`](browser-proof/SKILL.md) | Runs the real user journey with candidate, screenshot, console, network, and cleanup evidence |
| [`polish`](polish/SKILL.md) | Freezes scope, proves the baseline, simplifies changed files, reviews, fixes, reverifies, and fails closed before shipping |
| [`release-proof`](release-proof/SKILL.md) | Binds source, CI, artifact, runtime, and consumer proof to one exact candidate |

## Pull requests, loops, and communication

| Skill | What it does |
| --- | --- |
| [`pr-manager`](pr-manager/SKILL.md) | Resolves exact repo, head, and base, then drafts, creates, updates, or monitors a PR or MR with explicit write authority |
| [`watch-loop`](watch-loop/SKILL.md) | Runs one fresh idempotent tick with a cursor, quiet path, retry cap, and stop condition |
| [`babysit-prs`](babysit-prs/SKILL.md) | Applies the watch-loop pattern to new commits, comments, review state, CI, conflicts, and merge readiness |
| [`whatsapp-me`](whatsapp-me/SKILL.md) | Sends real proof-based OpenClaw updates to a configured WhatsApp target and routes replies to the right task, session, and worktree |

## Project maintenance

| Skill | What it does |
| --- | --- |
| [`sync-project`](sync-project/SKILL.md) | Fetches, prunes, and reports single or multi-repo status without resetting dirty work |
| [`status-report`](status-report/SKILL.md) | Builds daily, weekly, monthly, or custom reports from git, tickets, docs, meetings, email, and calendars |
| [`cleanup`](cleanup/SKILL.md) | Removes only clearly owned disposable state and reports dirty, shared, or ambiguous leftovers |

## Common flows

### Start any task

```text
workflow
  -> context, when outside information matters
  -> the concrete lane skill
  -> worktree and dev-flow, when code changes
  -> polish
  -> release-proof or cleanup
```

### Bug

```text
workflow -> context -> bug-repro -> bugfix -> polish -> release-proof -> cleanup
```

### Pull request babysitter

```text
watch-loop scheduler -> babysit-prs tick -> whatsapp-me only when something meaningful changed
```

`whatsapp-me` is also called directly by any running skill when the user asked to be notified away from the computer. It does not watch or schedule anything itself.

### Ticket or documentation work

```text
workflow -> context -> jira-manager or docs-manager -> preview -> authorized write -> verified read-back
```

## Install

List the skills first:

```bash
npx skills add sabirmgd/terminal-first-goal-loop --list
```

Install one skill globally for Claude Code and Codex:

```bash
npx skills add sabirmgd/terminal-first-goal-loop \
  --skill workflow \
  --global \
  --agent claude-code \
  --agent codex
```

Install the full pack only after reviewing it:

```bash
npx skills add sabirmgd/terminal-first-goal-loop \
  --skill '*' \
  --global \
  --agent claude-code \
  --agent codex
```

The installer can link both agents to one canonical copy. That is better than maintaining separate versions that drift.

## Private adapters

The public skills do not contain real repository paths, ticket prefixes, accounts, tenants, clusters, credentials, test URLs, release commands, or notification targets.

Keep those details in private project guidance. The skill defines the workflow; the private adapter tells it how the current project performs that workflow.

See the [community and native ecosystem](../docs/ecosystem.md), [tooling catalog](../docs/tooling.md), and [public safety policy](../docs/publishing-policy.md).
