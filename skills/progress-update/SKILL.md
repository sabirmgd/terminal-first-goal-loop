---
name: progress-update
description: Report progress from verified milestones through notifications, chat, or WhatsApp without sending noise.
---

# Progress Update

Use this when long-running work should report back without a human polling the terminal.

## Progress basis

Progress should come from completed verified milestones, not elapsed time.

Examples:

- repro complete
- failing test written
- focused tests green
- browser proof captured
- review findings resolved

## Update format

Prefer:

```text
<percent>% complete
Done: <verified milestone>
Proof: <test count, screenshot, CI, or other evidence>
Next: <next milestone>
Blocked: <none or explicit blocker>
```

## Delivery

Possible delivery surfaces:

- local terminal notification
- cmux notification
- chat message
- WhatsApp update through OpenClaw

Use the shortest useful route. Do not send the same update twice with no new proof.

## Rules

1. Do not invent percentages.
2. Do not send raw logs when one sentence will do.
3. Do not say "still working" with no new evidence.
4. Distinguish done, next, and blocked clearly.

## Stop rules

Stop when:

- the new milestone has been reported
- nothing changed and the quiet path applies
