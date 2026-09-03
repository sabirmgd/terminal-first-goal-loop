# Skill Catalog

This public repo now focuses on the stable skill files that are concrete enough to be useful and generic enough to share.

## Concrete operating skills

These are the most practical skills in the pack.

| Skill | Purpose |
| --- | --- |
| [`jira-manager`](../skills/jira-manager/SKILL.md) | Retrieve, search, draft, create, update, and link tickets from the terminal |
| [`docs-manager`](../skills/docs-manager/SKILL.md) | Retrieve, draft, update, and publish wiki docs with source grounding and version checks |
| [`pr-manager`](../skills/pr-manager/SKILL.md) | Manage PR or MR drafts, updates, monitoring, and exact repo/head/base resolution |
| [`sync-project`](../skills/sync-project/SKILL.md) | Fetch and report multi-repo status without disturbing dirty trees |
| [`status-report`](../skills/status-report/SKILL.md) | Build daily, weekly, or monthly reports from real evidence across tools |
| [`babysit-pr`](../skills/babysit-pr/SKILL.md) | Run a quiet review or CI watch loop for one PR or MR |
| [`progress-update`](../skills/progress-update/SKILL.md) | Send proof-based milestones through notifications or WhatsApp |

## Supporting workflow skills

These support the concrete operating layer.

| Skill | Purpose |
| --- | --- |
| [`access-check`](../skills/access-check/SKILL.md) | Prove access safely without printing secrets |
| [`worktree-slot`](../skills/worktree-slot/SKILL.md) | Isolate mutable work into a safe worktree and slot |
| [`dev-flow`](../skills/dev-flow/SKILL.md) | Keep heavy infrastructure warm and give each task a predictable local lane |
| [`hot-reload`](../skills/hot-reload/SKILL.md) | Climb the proof ladder from the smallest failing boundary |
| [`browser-proof`](../skills/browser-proof/SKILL.md) | Capture user-facing proof with screenshots, console, network, and cleanup |
| [`review-and-polish`](../skills/review-and-polish/SKILL.md) | Separate maker, reviewer, simplifier, and verifier roles |
| [`release-proof`](../skills/release-proof/SKILL.md) | Bind source, CI, artifact, runtime, and consumer proof to one candidate |
| [`cleanup`](../skills/cleanup/SKILL.md) | Close work safely without deleting ambiguous state |

## Community tools I use

These are linked, not copied.

| Tool | Why it matters |
| --- | --- |
| [Ponytail](https://github.com/DietrichGebert/ponytail) | Pushes implementation toward the simplest code that still works |
| [PR Review Toolkit](https://github.com/anthropics/claude-plugins-official/blob/main/plugins/pr-review-toolkit/README.md) | Multi-agent code review from several angles |
| [Code Review Plugin](https://github.com/anthropics/claude-code/blob/main/plugins/code-review/README.md) | Another review surface for pull request analysis |
| [code-simplifier](https://github.com/anthropics/claude-plugins-official/blob/main/plugins/code-simplifier/agents/code-simplifier.md) | Simplifies changed files without changing behavior |
| [Impeccable](https://github.com/pbakaus/impeccable) | UI critique, refinement, and anti-generic design workflow |
| [Taste skill](https://github.com/leonxlnx/taste-skill) | Useful for stronger aesthetic direction in UI work |
| [agent-browser](https://github.com/vercel-labs/agent-browser) | Real browser proof for user-facing work |
| [OpenClaw](https://github.com/openclaw/openclaw) | Route progress and replies to chat channels such as WhatsApp |

## Native capabilities

I rely on native agent features too, but I do not copy them into this repo.

### OpenAI Codex

- [Customization overview](https://developers.openai.com/codex/customization/overview)
- [Plugins](https://developers.openai.com/codex/plugins)
- [Codex CLI](https://developers.openai.com/codex/cli)
- [Automations](https://developers.openai.com/codex/automations)
- [Memories](https://developers.openai.com/codex/memories)

### Claude Code

- [Claude Code skills](https://docs.anthropic.com/en/docs/claude-code/skills)
- [Claude Code goals](https://docs.anthropic.com/en/docs/claude-code/goal)
- [Claude Code hooks](https://docs.anthropic.com/en/docs/claude-code/hooks-guide)
- [Claude Code plugins](https://github.com/anthropics/claude-plugins-official)

## What stays private

The public repo does not include:

- project names and ticket prefixes
- private accounts, clusters, domains, or tenants
- credentials or secret recovery paths
- organization-specific release commands
- private workspace screenshots
