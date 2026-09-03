---
name: context
description: Gather a small, sourced context pack for one software task from tickets, docs, design, email, meeting transcripts, code, and history. Use before planning or implementation when missing context would cause guessing.
---

# Context

Use this skill to bring the right context into the task without flooding the agent with everything the organization knows.

Context work is read-only by default. It ends with a brief, not an implementation.

## Inputs

Identify:

- the task or question;
- the affected user or operator;
- the project or repository;
- the time range when recency matters;
- the sources that are actually relevant;
- any company policy that limits email, meeting, customer, or production data.

Do not require every source. A code-only task may need no ticket or meeting context.

## Source order

Use the cheapest authoritative source first:

1. Current code and git history for implemented behavior.
2. Ticket tracker for requested behavior and acceptance criteria.
3. Documentation for decisions, architecture, and operating rules.
4. Design source for user journeys and intended states.
5. Email or meeting transcript for missing business context.
6. Runtime or logs when the question is about live behavior.

Tickets and documents explain intent. Code and runtime prove what exists.

## Capability routing

Use an installed concrete skill when it exists:

- `jira-manager` for Jira tickets, comments, links, and history;
- `docs-manager` for Confluence or another approved wiki;
- a configured Figma MCP or CLI for frames, variables, and screenshots;
- a configured Gmail or mail connector for narrow read-only searches;
- a configured Fireflies or meeting connector for approved transcripts;
- git and repository tools for code, history, and current state.

These are semantic handoffs, not hard dependencies. If a named skill is missing but the equivalent approved tool is available, follow the same boundary directly. If no safe source is available, record the gap instead of inventing context.

When the task needs source-specific handling, read [references/sources.md](references/sources.md). Load only the sections for the sources being used.

## Narrow each source

### Tickets

Fetch the named issue or a narrow search. Keep the summary, description, acceptance criteria, status, linked work, and decisions that affect the task. Do not copy an entire backlog.

### Documentation

Fetch the named page or the smallest relevant section. Record the page title, version or last update, and whether code has moved since it was written.

### Design

Fetch the relevant frame or journey, not the whole design file. Record the frame identity, visible states, important copy, empty/error states, and anything that conflicts with the implementation.

### Email

Define the account, search, labels or participants, and date range before reading. Keep only facts and decisions that change the task. Do not store unrelated messages or entire mailboxes.

### Meeting transcripts

Use only when policy, notice, consent, and access allow it. Extract decisions, action items, unresolved questions, and useful quotes. Keep the transcript as source evidence, not as the public or durable document.

### Code and runtime

Trace the actual files, callers, configuration, tests, and running behavior needed to validate claims from other sources.

## Resolve conflicts

When sources disagree:

1. Name the contradiction.
2. Prefer current executable behavior for what exists today.
3. Prefer an explicit recent human decision for what should change.
4. Mark stale or inferred claims.
5. Ask for a decision only when the conflict changes the outcome materially.

Do not silently blend incompatible sources into one confident summary.

## Output contract

```text
CONTEXT PACK | task=<label> | freshness=<current|mixed|stale>
OUTCOME | <what the task is trying to change>
USER | <affected user or operator>
FACTS | <sourced facts that matter>
DECISIONS | <confirmed decisions>
CURRENT BEHAVIOR | <code or runtime evidence>
CONSTRAINTS | <security, policy, compatibility, data, or release constraints>
CONTRADICTIONS | <none or explicit conflicts>
OPEN QUESTIONS | <only questions that change the plan>
SOURCES | <source, identity, date or version>
NEXT | <shape goal, reproduce bug, review, or other next skill>
```

## Safety

- Never print or store secret values.
- Redact tokens, cookies, passwords, private keys, personal data, and customer content that the task does not need.
- Keep email and transcript evidence private unless publication is separately approved.
- Do not create or update tickets, documents, email, or design files during context capture.
- Do not claim a source was checked when the operation failed.

## Stop conditions

Stop when the context pack is sufficient to plan or investigate without guessing.

Also stop when:

- the next source requires unavailable access;
- policy or consent is unclear;
- the remaining question is a human decision;
- further retrieval would add volume but not change the task.
