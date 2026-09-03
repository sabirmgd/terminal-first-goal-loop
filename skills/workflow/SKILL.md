---
name: workflow
description: Run the real front door for terminal-first work. Use when a task starts as a message, ticket, screenshot, voice dump, review request, bug report, or half-formed idea and needs to become a scoped lane with the right context, proof, worktree, model, and stop rules before external actions.
---

# Workflow

This is the front door.

Use it when the task is still forming and the agent needs the right lane, context, proof, isolation, and authority before real execution starts.

It owns:

- task intake
- lane selection
- context prefetch
- proof definition
- human gate detection
- worktree and runtime isolation choice
- model and role choice
- routing to the next concrete workflow

## Start here

Before planning or editing:

1. Read the local guidance that governs the current repository.
2. Inspect current git state, active worktrees, and whether the checkout is dirty.
3. Refresh current source truth when the task depends on a moving target such as a branch, PR, issue, design, inbox, or deployment.
4. Treat stale chat memory as a clue, not authority.

If the repository has its own sync or refresh ritual, use it. Otherwise use the lightest safe equivalent such as `git fetch`, `git status`, and `git worktree list`.

## Choose one lane

Every task gets one primary lane first:

- discovery and design
- feature delivery
- bug reproduction
- bug fixing
- review and polish
- operations and release
- integrations and tooling
- knowledge and communication
- maintenance and simplification

If the overall initiative spans several lanes, pick the first lane that should happen now.

## Pull only the context that matters

Possible sources:

- repository code, tests, and local guidance
- issue tracker tickets and comments
- docs or wiki pages
- design files or screenshots
- email threads
- call notes or transcript summaries
- logs, traces, or saved repro evidence
- current PR or review discussion

Pull a source only when it changes the decision.

## Provenance and freshness rules

For every important fact, keep three things visible:

- source
- freshness or date
- whether it is confirmed behavior or only intent

When sources disagree:

1. prefer current measured behavior over stale recollection
2. prefer the latest approved decision over an older draft
3. keep the disagreement visible when it changes implementation or risk

Do not hide uncertainty by writing a smoother summary.

## Read-only versus write authority

Default posture:

- reading, planning, local analysis, and local non-destructive proof may proceed
- local code mutation may proceed when the user asked for delivery and the scope is clear
- external writes do not proceed without explicit authority

External writes include tickets, docs, comments, PR updates, merge, deploy, release, rollback, traffic changes, and destructive cleanup outside clearly owned local scratch state.

If a task is read-only, keep it read-only all the way through the lane.

## Define proof before launch

Every task needs an honest proof path.

Possible proof levels:

- static or type check
- unit or component test
- integration or API test
- local hot-reload request
- isolated slot or sandbox proof
- browser journey
- exact runtime or deployed proof

Pick the cheapest proof that still proves the claim. Do not make the most expensive proof the default inner loop if a cheaper one can expose the same failure.

## Pick the execution shape

Execution shapes:

- direct loop: one surface, fast proof, likely single-agent
- bounded goal: long-running work with milestones and low human touch
- watch loop: repeated state checks with a cursor and quiet path

## Worktree and runtime isolation

If the task will change code and other mutating work may run in parallel:

- create or use an isolated worktree
- branch from a clean, current base
- keep the main checkout untouched if it is dirty

If the stack has expensive startup, keep shared heavy services warm, give each mutating task its own slot or isolated runtime surface, and stop only the slot when that task ends.

Never let two mutating agents edit the same active checkout.

## Choose the role, then the model

Pick the role first:

- fast reader for retrieval and inventory
- strong builder for implementation
- deep reasoner for architecture or ambiguous bugs
- independent reviewer for correctness and risk
- verifier for final proof

Then pick the lightest model that fits that role.

## Route the task

Feature delivery:

- confirm user journey
- pull design if it exists
- define acceptance proof
- isolate code changes
- hand off to `deliver-feature`

Bug reproduction:

- translate report into expected behavior
- choose the narrowest proof surface
- keep it read-only
- hand off to `bug-repro`

Bug fixing:

- carry the repro evidence forward
- define the durable failing test home
- isolate the worktree
- define shipped proof
- hand off to `bugfix`

Review and polish:

- route here when implementation works and the job is now review, simplification, and signoff proof

Operations and release:

- route here when the real question is deployed identity, rollout state, health, traffic, or exact runtime proof

Integrations and tooling:

- route here when a repeated human step should become a CLI, MCP-backed action, API wrapper, or deterministic script

Knowledge and communication:

- route here when the task is to turn work into a ticket, document, handoff, saved decision, or progress update

Keep the source and authority visible. Do not make external writes implicitly.

## Human gates

Stop and hand back when any of these are true:

- expected behavior is genuinely unclear
- there are several valid fixes with different product outcomes
- proof depends on missing access or unavailable runtime
- the next step is an external write without explicit authority
- destructive cleanup would touch ambiguous or unowned state
- a release, merge, deploy, or rollback decision is still human-owned

## Output contract

Return a compact start brief:

```text
WORKFLOW | lane=<lane> | execution=<direct|goal|loop>
OUTCOME | <plain-language result>
CONTEXT | <only the sources that matter, with freshness>
PROOF | <the proof path that will make this real>
ISOLATION | <none or branch/worktree/slot decision>
ROLES | <maker, reviewer, verifier, or other needed roles>
HUMAN GATE | <none or exact decision>
NEXT | <next skill or next concrete action>
STOP | <what should pause the workflow>
```

If the task is large, add milestones so later percentage updates can be tied to proof instead of optimism.
