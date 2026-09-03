---
name: jira-manager
description: Retrieve, search, draft, create, update, and link issue-tracker tickets through the available CLI, MCP, or API, with read-only default behavior and explicit write previews.
---

# Jira Manager

Use this when a task needs ticket context or when a ticket should be drafted or updated from the terminal.

## Default mode

Read-only by default.

That means it is always safe to:

- retrieve one ticket
- search tickets
- list comments or linked issues
- draft ticket text
- prepare an update payload

External writes need explicit authority.

## Interface order

Prefer:

1. the configured issue-tracker CLI
2. a scoped MCP tool
3. a direct API call

Do not open the browser unless the task is blocked on an interface gap.

## Read workflow

For read operations, return:

```text
System:
Project or workspace:
Ticket:
Summary:
Status:
Assignee:
Links:
Comments or activity summary:
Evidence:
```

When searching, return the smallest useful result set. Do not paste the whole board.

## Draft workflow

When asked to create or update a ticket, first produce a preview with:

```text
Action:
Target project or ticket:
Proposed title:
Proposed description:
Proposed acceptance criteria:
Proposed links or attachments:
Exact write command or payload:
```

Do not send the write yet unless the current task explicitly authorizes it.

## Write workflow

Once authority is explicit:

1. resolve the exact project and ticket target
2. send the smallest required create or update
3. read the result back
4. return the new ticket key, status, and proof

If the write fails, show the failure plainly. Do not quietly retry with changed fields.

## Rules

1. Never invent ticket activity or acceptance criteria.
2. Never move a ticket, add a comment, or change an assignee without explicit authority.
3. Preserve issue links, dependencies, and external identifiers exactly.
4. If the task describes work discussed in chat, draft the ticket text first instead of creating a vague ticket directly.

## Stop rules

Stop when:

- the read result is complete
- the preview is ready for approval
- the authorized write is confirmed
- the target system or permission is blocked and the blocker is explicit
