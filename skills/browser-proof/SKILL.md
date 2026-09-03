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

## Prove The Real Boundary

Use the consumer that matters:

- UI feature: real browser journey.
- CLI feature: actual CLI command and normal authentication path.
- API-only contract: real client or exact request shape, clearly labeled as API proof.

Do not replace a click with a database update or curl request when the claim is about the user journey. Lower-level checks can explain the result, but they do not become UI proof.

Test the failure boundary that is most likely to hide the defect: wrong role, empty state, validation failure, stale session, duplicate action, page refresh, responsive viewport, or multi-instance delivery when relevant.

## Evidence Rules

1. Tie the screenshot, console, and network evidence to the same run and time window.
2. Record whether the event was produced through the real upstream flow or injected to test a downstream contract.
3. Do not describe a contract injection as full end-to-end automation.
4. Redact credentials and customer data while keeping candidate, time, status, and resource identity useful.
5. If one layer is unavailable, name the missing proof and lower the verdict.

## Cleanup

Close the browser session created for the proof. Remove only task-owned temporary auth, test data, downloads, recordings, and port forwards. If cleanup fails, return `blocked` and list the residual state even when the visible assertion passed.

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

## Stop Conditions

Stop with:

- `pass` when the real action produced the expected visible result and cleanup is complete;
- `fail` when the journey contradicted the claim;
- `blocked` when candidate identity, access, browser operation, required evidence, or cleanup cannot be proved.
