---
name: prepare-context
description: Pull only the task-relevant context from tickets, docs, design, email, transcripts, code, and runtime state. Use when the next lane needs a small, sourced context pack instead of a large dump.
---

# Prepare Context

The point is not to collect everything. The point is to collect what this task
needs, say where it came from, and stop.

## Workflow

1. Name the lane.
2. List the minimum sources needed for that lane.
3. Pull the smallest useful slice from each source.
4. Separate outcome, current behavior, constraints, proof inputs, and open questions.
5. Stop once the next skill can work without guessing.

## Rules

1. Pull only what matters to the current task.
2. Keep source and freshness visible.
3. Keep email and transcripts read-only by default.
4. Prefer a concise context pack over a raw dump.

## Output Contract

```text
CONTEXT PACK | lane=<name> | sources=<count>
OUTCOME | <plain-language goal>
CURRENT | <current behavior or issue>
CONSTRAINTS | <important limits>
PROOF INPUTS | <tests, screens, docs, data, or tickets>
OPEN QUESTIONS | <none or exact unknowns>
```
