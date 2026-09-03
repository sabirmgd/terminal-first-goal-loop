# Context Source Rules

Read only the sections for the sources the current task needs.

## Jira or another ticket tracker

Prefer the `jira-manager` skill when installed.

Collect:

- key, summary, description, status, assignee;
- acceptance criteria;
- linked issues, blockers, comments, and recent changes;
- attachments or external references that materially change the task.

Do not create, update, transition, assign, or comment during context capture. Keep write actions in the ticket-management workflow with an explicit preview and authority check.

## Confluence or another documentation system

Prefer the `docs-manager` skill when installed.

Collect:

- page title and location;
- current version and last update;
- relevant decisions, constraints, diagrams, and runbooks;
- contradictions with current code or newer decisions.

Do not treat a document as current merely because it exists. Record its freshness and compare important claims with code or runtime.

## Figma or another design source

Use the configured design MCP or CLI.

Collect:

- exact file and frame identity;
- screenshot or design context for the relevant journey;
- states, interactions, copy, permissions, empty states, and errors;
- tokens or variables only when implementation needs them.

Avoid pulling a whole design file for one screen. Design context is intent, not proof that the implementation matches.

## Gmail or another mailbox

Use a configured approved connector in read-only mode.

Define before searching:

- account;
- query, participants, labels, or thread;
- date range;
- task reason;
- what may be retained.

Summarize only facts, decisions, and attachments that change the task. Do not copy unrelated personal or customer messages into project memory.

## Fireflies or another meeting transcript source

Confirm policy, notice, consent, access, and retention before retrieval.

Collect:

- meeting identity and date;
- attendees only when needed;
- confirmed decisions;
- actions and owners;
- open questions;
- short supporting excerpts when necessary.

Do not publish the raw transcript by default. Store the curated note with provenance and keep the source governed by its original access policy.

## Git and source code

Collect:

- current branch and base;
- relevant files and callers;
- tests that define current behavior;
- history that explains a non-obvious decision;
- existing utilities or patterns that should be reused.

Code is the source of truth for what exists. It is not automatically the source of truth for what the product should do next.

## Runtime and logs

Use only when the task asks about deployed or observed behavior.

Collect:

- environment and exact candidate identity;
- bounded time range;
- correlation or resource identity;
- the smallest useful log or state excerpt;
- whether the observation is direct proof or inference.

Redact secrets and customer content. Do not describe a different environment or candidate as proof for the target.
