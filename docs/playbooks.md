# Playbooks

The article explains the ideas. This file turns them into repeatable flows.

## 1. Shape and launch a task

Spend a short focused session defining the task before asking an agent to run for hours.

```text
Outcome: What changes for the user or operator?
Context: Which code, tickets, docs, emails, or designs matter?
Proof: What should fail before the work and pass afterward?
Decisions: Which choices still need a human?
Stop: What should make the agent pause?
```

```mermaid
flowchart LR
    I["Raw idea"] --> C["Gather only relevant context"]
    C --> O["Define the outcome"]
    O --> P["Define the proof"]
    P --> G["Launch the goal"]
    G --> H["Return only for proof or a decision"]
```

## 2. Feature delivery

```mermaid
flowchart TB
    U["Start with the user journey"] --> D{"Design exists?"}
    D -- Yes --> F["Load design context"]
    D -- No --> A["Create a disposable HTML artifact"]
    F --> P["Plan architecture, data, permissions, and failure states"]
    A --> P
    P --> T["Write acceptance tests"]
    T --> W["Create worktree and slot"]
    W --> B["Build the smallest useful slice"]
    B --> V["Integration, API, and browser proof"]
    V --> R["Review, simplify, and verify"]
    R --> H["Prepare handoff or pull request"]
```

Notes:

- Start from the user journey, not only the backend shape.
- If the UI is unclear, create something disposable that lets you see it.
- The proof should live in the repository when possible.

## 3. Bug investigation

```mermaid
flowchart TB
    C["Bug claim"] --> R["Reproduce the claim"]
    R --> X{"Reproduced?"}
    X -- No --> N["Report what happened and stop"]
    X -- Yes --> T["Trace the root cause"]
    T --> O["List fix options and risks"]
    O --> D["Confirm intended behavior if needed"]
    D --> B["Write the decision-ready brief"]
```

Investigation is a real lane. It is not just the first paragraph of a bug fix.

## 4. Bug fixing

```mermaid
flowchart TB
    C["Approved bug or current repro bundle"] --> T["Write a failing durable test"]
    T --> F["Fix the smallest shared cause"]
    F --> L["Run the same test locally or in a slot"]
    L --> R["Review and simplify"]
    R --> P["Prove the final result"]
```

Notes:

- Reproducing the bug is not enough. The fix needs a durable test.
- The smallest fix should live at the shared cause when possible.
- Local green is not shipped proof when the environment matters.

## 5. Worktrees, warm infrastructure, and slots

```mermaid
flowchart TB
    R["One repository"] --> WA["Worktree A"]
    R --> WB["Worktree B"]
    R --> WC["Worktree C"]

    I["Shared warm infrastructure"] --> IA["Database"]
    I --> IB["Cache and queues"]
    I --> IC["Storage and support services"]

    WA --> SA["App slot A"]
    WB --> SB["App slot B"]
    WC --> SC["App slot C"]
```

The operating rules are simple:

- shared infrastructure stays warm
- each mutable task gets its own slot
- stopping a slot must not stop shared services
- cleanup should release slot state, browser state, worktree state, and disposable test data

## 6. Cheapest useful test ladder

```mermaid
flowchart LR
    E["Exact failure"] --> T0["Static or focused test"]
    T0 --> T1["Integration or API check"]
    T1 --> T2["Hot-reload or slot request"]
    T2 --> T3["Browser journey"]
    T3 --> T4["Runtime or deployed proof"]
```

Rules:

- do not jump to the most expensive proof first
- do not rerun the full stack after every tiny edit
- if a higher level fails, drop back to the cheapest level that reproduces that failure

## 7. Maker, reviewer, simplifier, verifier

```mermaid
sequenceDiagram
    participant H as Human
    participant M as Maker
    participant R as Reviewer
    participant S as Simplifier
    participant V as Verifier

    H->>M: Tight goal and boundaries
    M->>M: Build and run focused proof
    M->>R: Exact diff and tested candidate
    R-->>M: Confirmed findings
    M->>S: Fix and trim changed files
    S-->>V: Final candidate
    V-->>H: Proof, risk, and next action
```

## 8. Context capture

```mermaid
flowchart LR
    S["Task"] --> J["Ticket or issue"]
    S --> D["Docs or design"]
    S --> E["Email or transcript"]
    S --> C["Code and history"]
    J --> P["One working brief"]
    D --> P
    E --> P
    C --> P
```

Pull only what the task needs from systems like Jira, Confluence, Figma, Gmail, meeting transcripts, code, and git history.

## 9. Loops

Every loop should run in small fresh ticks.

```mermaid
flowchart LR
    S["Scheduler or live loop"] --> T["Fresh tick"]
    T --> P["Read saved cursor"]
    P --> N["Read current state"]
    N --> D{"Changed?"}
    D -- No --> Q["Quiet exit"]
    D -- Yes --> A["Allowed action"]
    A --> V["Verify action"]
    V --> C["Save new cursor"]
    C --> U["Notify if useful"]
```

### Useful loop types

| Loop | One tick does | Typical stop condition |
| --- | --- | --- |
| Pull request babysitter | Check new commits, comments, CI, review state, and mergeability | Closed, merged, or human decision |
| CI or deploy watcher | Poll one run tied to one candidate | Success, failure, timeout, or new candidate |
| Bug investigation loop | Keep narrowing a repro or root cause inside approved boundaries | Root-cause brief or human decision |
| Context prefetch loop | Pull new ticket, doc, or transcript context for tomorrow's work | Brief captured or source unchanged |
| Dependency wait | Check one named outside condition | Condition changed or wait cancelled |
| Cleanup loop | Remove safe disposable resources | Everything cleaned or ambiguity found |

### Local loops and scheduled loops

Local loops are useful when the work depends on local files, local CLIs, a signed-in browser session, or the current worktree. That is where leaving the laptop open and letting a live agent session keep running makes sense.

Scheduled loops are useful when the task should start at a known time or recur predictably. A cloud schedule does not automatically inherit local files, keychains, CLI auth, or browser sessions.

## 10. Proof-based progress

The percentage should reflect completed milestones, not elapsed time.

```mermaid
flowchart LR
    G["Goal milestones"] --> P["Completed verified milestones"]
    P --> M["Progress message"]
    M --> N["cmux notification"]
    M --> O["OpenClaw"]
    O --> W["WhatsApp update"]
```

Useful format:

```text
65% complete
Done: integration tests pass
Proof: 18/18
Next: browser journey
Blocked: none
```
