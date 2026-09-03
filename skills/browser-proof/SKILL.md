---
name: browser-proof
description: Prove a user-facing claim in the actual browser with exact candidate context, the real user action, screenshots, console and network checks, and cleanup. Use when the thing being claimed is visible behavior, not just backend output.
---

# Browser Proof

Use the browser when the claim is about what a user sees or does.

## Core Rules

1. Name the exact candidate before starting.
2. Use the real entry path and the real user action.
3. Save screenshot evidence from the same run that proves the result.
4. Check console and relevant network traffic around the target action.
5. Never substitute an API-only proof for a UI claim.
6. Clean up temporary browser state created only for the proof.

## Workflow

1. Record the candidate identity: SHA, build, revision, preview, slot, or URL.
2. State the exact claim being proved.
3. Open the real page or flow.
4. Clear old console and network noise if the tool supports it.
5. Perform the real action.
6. Capture:
   - visible result
   - screenshot path
   - relevant console state
   - relevant network result
7. Report `pass`, `fail`, or `blocked`.

## Output Contract

```text
BROWSER PROOF | claim=<summary> | verdict=<pass|fail|blocked>
CANDIDATE | <sha|build|revision|slot|url>
ACTION | <real user action>
RESULT | <visible outcome>
SCREENSHOT | <path-or-artifact>
CONSOLE | <clean|warning|error summary>
NETWORK | <relevant request summary>
CLEANUP | <done|not-needed|blocked>
```
