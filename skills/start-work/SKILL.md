---
name: start-work
description: Classify a new request, choose the right work lane, identify the first real gate, and hand off to the next skill. Use when the task is still rough and the main job is to start correctly instead of jumping straight into code.
---

# Start Work

Use this skill at the front door.

## Workflow

1. Restate the task in one sentence.
2. Choose one lane:
   - design
   - feature
   - bug
   - review
   - release
   - automation
   - docs or reporting
3. Name the first thing that must be true before implementation.
4. Pull only the first missing context.
5. Hand off to the next skill and stop.

## Rules

1. Do not start coding while the lane is still unclear.
2. If access is the blocker, prove access first.
3. If the task is a bug report, reproduce it before fixing it.
4. If the task is large, shape the goal before the build starts.

## Output Contract

```text
START WORK | lane=<name>
TASK | <one-sentence restatement>
FIRST GATE | <what must be true next>
NEXT SKILL | <skill name>
```
