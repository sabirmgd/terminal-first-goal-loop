---
name: status-report
description: Build daily, weekly, or monthly status reports from evidence across git, tickets, docs, meetings, email, and calendars, with visible source failures and no invented activity.
---

# Status Report

Use this when a progress report should be grounded in what actually happened.

## Sources

Possible sources:

- git history and diffs
- pull requests or merge requests
- tickets and ticket comments
- docs or wiki updates
- meeting notes or transcripts
- email
- calendar

Use only the sources that matter to the requested report.

## Time window

Resolve the exact reporting window first:

- daily
- weekly
- monthly
- custom range

Do not mix older work into the current report because it sounds relevant.

## Build from evidence

For each claimed item, keep at least one real source.

Good report items:

- merged PR with link
- ticket moved with status proof
- page published with version proof
- meeting decision with transcript or notes source
- deployment or verification with concrete evidence

## Output shape

Return:

```text
Window:
Sources checked:
Completed work:
In progress:
Blocked:
Follow-ups:
Source failures:
```

If the user wants a specific format, keep this evidence model underneath it.

## Rules

1. Never invent work that has no source.
2. Deduplicate the same activity across git, tickets, and docs.
3. Keep source failures visible. Missing Jira or email access should not become silent omissions.
4. Separate completed work from discussion, planning, or open investigation.
5. If a period was quiet, say it was quiet.

## Stop rules

Stop when:

- the report is grounded and complete for the requested window
- a missing source leaves a visible gap that cannot be closed safely
