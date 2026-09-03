# Ecosystem

This repo separates three kinds of capabilities:

- first-party skills shipped here
- community skills and tools maintained elsewhere
- native capabilities already built into the coding agent

That separation keeps the public core clean.

## First-Party Skills

The reusable workflow skills maintained here use one front door, `workflow`, plus concrete capabilities for context, Jira, docs, bug work, worktrees, dev slots, hot reload, browser proof, polishing, pull requests, loops, OpenClaw and WhatsApp delivery, release proof, reporting, sync, access, and cleanup.

See the complete [first-party skills catalog](../skills/README.md).

Figma, Gmail, Fireflies, and similar account-backed connectors are not copied as public skills. The public `context` skill can use them when they are configured, while the real account and permission details remain private.

## Community Tools and Skills

These are valuable, but they should stay upstream.

| Project | Use in this workflow | Source |
| --- | --- | --- |
| Superpowers | planning, disciplined build flows, debugging patterns, review flows | [obra/superpowers](https://github.com/obra/superpowers) |
| Ponytail | fewer files, less code, standard-library-first pressure | [DietrichGebert/ponytail](https://github.com/DietrichGebert/ponytail) |
| Taste | stronger visual direction for frontend work | [Leonxlnx/taste-skill](https://github.com/Leonxlnx/taste-skill) |
| Impeccable | UI critique, polish, accessibility, stronger taste | [pbakaus/impeccable](https://github.com/pbakaus/impeccable) |
| agent-browser | real browser journeys, screenshots, console and network proof | [vercel-labs/agent-browser](https://github.com/vercel-labs/agent-browser) |
| oh-my-codex | Codex roles, teams, workflows, state, and orchestration | [Yeachan-Heo/oh-my-codex](https://github.com/Yeachan-Heo/oh-my-codex) |
| Skills CLI | install one skill pack into several agent runtimes | [vercel-labs/skills](https://github.com/vercel-labs/skills) |
| Agent Skills collection | reference implementations and reusable public skills | [vercel-labs/agent-skills](https://github.com/vercel-labs/agent-skills) |
| OpenClaw | route milestone updates and replies through chat surfaces such as WhatsApp | [openclaw/openclaw](https://github.com/openclaw/openclaw) |
| Apiify | turn browser-only workflows into deterministic scripts when possible | [sabirmgd/apiify-skills](https://github.com/sabirmgd/apiify-skills) |

## Native Claude Code Capabilities

These are runtime features, not repo-owned copies:

| Capability | Why it matters |
| --- | --- |
| routines and schedules | run recurring work with explicit environment boundaries |
| local loops | keep a local task ticking while the laptop and session stay alive |
| plan mode | slow down before high-impact changes |
| skills | load workflow instructions only when relevant |
| subagents | separate maker, reviewer, and verifier contexts |
| hooks | enforce local rules or emit lifecycle events |
| plugins | install maintained skill bundles |
| [PR Review Toolkit](https://github.com/anthropics/claude-plugins-official/blob/main/plugins/pr-review-toolkit/README.md) | run specialized review passes over code, tests, types, comments, and silent failures |
| [code-simplifier](https://github.com/anthropics/claude-plugins-official/blob/main/plugins/code-simplifier/agents/code-simplifier.md) | simplify changed code without changing behavior |

## Native Codex Capabilities

| Capability | Why it matters |
| --- | --- |
| skills | run the same public `SKILL.md` pack |
| native subagents | split exploration, implementation, review, and verification |
| plans | track multi-step work without overbuilding orchestration |
| memory and notes | keep context durable across long tasks |
| MCP and apps | reach connected systems through structured tools |
| browser and artifact skills | build or verify sites, docs, visuals, and files |
| scheduled tasks | run supported routines with explicit scope and access |

## Inclusion Rule

A capability belongs in this repo only when all of these are true:

- it is reusable across projects
- it is safe to publish
- it has a clear trigger, proof model, and stop rule
- it is not better maintained upstream

Otherwise it should stay:

- upstream, if it is community or native
- private, if it contains project-specific operations
