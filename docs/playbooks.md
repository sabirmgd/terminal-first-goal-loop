# Playbooks

The main article explains the ideas. This file turns them into reusable flows.

## 1. Shape and launch a task

Spend a short, focused session defining the task before asking an agent to run for hours.

```text
Outcome: What changes for the user or operator?
Context: Which code, tickets, designs, documents, or messages matter?
Proof: What will fail before the work and pass afterward?
Decisions: Which choices still need a human?
Stop: What must make the agent pause?
```

```mermaid
flowchart LR
    I["Raw idea"] --> C["Gather only relevant context"]
    C --> O["Define outcome"]
    O --> P["Define proof"]
    P --> R["Review the plan if needed"]
    R --> G["Launch long-running goal"]
    G --> H["Return for decision or proof"]
```

## 2. Feature delivery

```mermaid
flowchart TB
    U["Start with the user journey"] --> D{"Design exists?"}
    D -- Yes --> F["Load Figma or design context"]
    D -- No --> A["Create a disposable HTML artifact"]
    F --> P["Plan architecture, data and failure states"]
    A --> P
    P --> T["Write acceptance tests"]
    T --> W["Create worktree and app slot"]
    W --> B["Build with hot reload"]
    B --> V["Integration and browser proof"]
    V --> Q["Review, simplify and verify"]
    Q --> PR["Prepare pull request and handoff"]
```

The final proof depends on the surface. Backend behavior normally needs a real API or integration test. User-facing behavior needs the actual browser journey and screenshots.

## 3. Bug fixing

```mermaid
flowchart TB
    C["Bug report"] --> R["Reproduce the claim"]
    R --> X{"Reproduced?"}
    X -- No --> S["Stop and report what was observed"]
    X -- Yes --> O["Trace the root cause"]
    O --> D["Explain choices and confirm intended behavior"]
    D --> T["Write a failing test"]
    T --> F["Fix the shared cause"]
    F --> L["Run the same test locally or in a slot"]
    L --> Q["Review, simplify and verify"]
    Q --> P["Ship the exact tested version"]
    P --> E["Run the same test in the affected environment"]
```

Investigation can run without a human when it is read-only. Fixing pauses only when the expected behavior or risk choice is genuinely unclear.

## 4. Worktrees, warm infrastructure, and slots

```mermaid
flowchart TB
    R["One repository"] --> WA["Worktree A"]
    R --> WB["Worktree B"]
    R --> WC["Worktree C"]

    I["Shared warm infrastructure"] --> IA["Database"]
    I --> IB["Cache and queues"]
    I --> IC["Storage and supporting services"]

    WA --> SA["App slot A and fixed ports"]
    WB --> SB["App slot B and fixed ports"]
    WC --> SC["App slot C and fixed ports"]

    IA --> SA
    IA --> SB
    IA --> SC
    IB --> SA
    IB --> SB
    IB --> SC
```

A slot should support five operations: start, list, smoke, stop, and release. Stopping an app slot must not stop shared infrastructure.

## 5. Cheapest-test loop

```mermaid
flowchart LR
    E["Exact failure"] --> T0["Static or unit test"]
    T0 --> T1["Component or integration test"]
    T1 --> T2["Hot-reload request"]
    T2 --> T3["Isolated slot or sandbox"]
    T3 --> T4["Real browser or consumer"]
    T4 --> T5["CI and deployed runtime"]
    T5 --> J["Full journey"]
```

Do not climb while the current level is red. When a higher level exposes a new error, return to the cheapest level that reproduces that error. Keep unrelated passing proof.

## 6. Maker, reviewer, simplifier, verifier

```mermaid
sequenceDiagram
    participant H as Human
    participant M as Maker
    participant R as Reviewer
    participant S as Simplifier
    participant V as Verifier

    H->>M: Reviewed goal and boundaries
    M->>M: Build and run focused tests
    M->>R: Exact diff and tested version
    R-->>M: Confirmed findings
    M->>M: Fix the smallest coherent set
    M->>S: Changed files only
    S-->>V: Simplified exact version
    V-->>H: Proof, remaining risk, and decision
```

The reviewer may discover issues. A verifier can confirm or reject those issues against the code. Verification should not become a new, unbounded review.

## 7. Running loops

Every recurring loop is a sequence of small, fresh ticks.

```mermaid
flowchart LR
    S["Scheduler"] --> T["Fresh tick"]
    T --> P["Read saved cursor"]
    P --> N["Read current state"]
    N --> D{"Changed?"}
    D -- No --> Q["Quiet exit"]
    D -- Yes --> A["Allowed action"]
    A --> V["Verify action"]
    V --> C["Save new cursor"]
    C --> U["Notify if useful"]
```

### Live local loop

Use a live loop when the task needs local files, local CLIs, signed-in sessions, or the current checkout.

```text
/loop 30m /loop-tick <project> babysit
```

The laptop must stay awake, the network must remain available, and the Claude Code session must keep running.

### Scheduled routine

Use `/schedule` when a task should start at a known time or recur without manually opening the same prompt. Confirm what environment the routine runs in. A cloud routine does not automatically inherit access to local files, keychains, CLIs, browser sessions, or worktrees.

### Useful loop types

| Loop | One tick | Stop condition |
| --- | --- | --- |
| Development | Run the smallest failing test and next safe fix | Candidate is ready or a decision is needed |
| Review and fix | Review the latest diff and recheck confirmed findings | Clean, no progress, or round limit |
| Pull request babysitter | Check new commits, comments, CI, and merge state | Closed, merged, or human decision |
| CI and deployment watcher | Poll one run tied to one commit | Success, failure, cancellation, timeout, or new commit |
| Dependency wait | Check one named outside condition | Condition changes or wait is cancelled |
| Runtime monitor | Compare health with the last snapshot | Meaningful change or scheduled digest |
| Knowledge capture | Turn one approved source into a short durable note | Note saved or external publication needs approval |
| Cleanup | Recheck and remove safe disposable resources | Everything removed or ambiguous state found |

Every loop needs saved state, idempotent actions, a quiet path, an authority limit, and a maximum retry or review count.

## 8. Progress updates

Define milestones before starting. The percentage is the weight of completed milestones, not a guess based on time.

```text
65% complete
Done: integration tests pass
Proof: 18/18
Next: browser journey
Blocked: none
```

Never report 100% while required runtime proof, cleanup, or a human decision remains open.
