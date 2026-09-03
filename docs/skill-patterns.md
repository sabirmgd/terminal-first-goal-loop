# Skill Design Rules

The skills in this repo are not generic prompt snippets. They are small operating contracts.

Each good skill answers a few concrete questions:

```text
When should this run?
What is it allowed to read?
What is it allowed to change?
What proof does it need?
What still needs a human?
When must it stop?
```

That is enough to make a workflow reusable without turning it into a giant framework.

## The recurring patterns

### 1. Start with the lane

The agent should know whether the task is discovery and design, feature delivery, bug investigation, bug fixing, review and hardening, operations and release, integrations and tooling, or knowledge and communication.

The lane changes the proof.

### 2. Gather only task-relevant context

A useful context skill narrows instead of dumping.

It should pull only the ticket, design, doc, email, transcript, or code context that matters and record where it came from.

### 3. Separate access discovery from secret mutation

An access skill should prove that a system is available and that the agent is looking at the right identity, account, project, or cluster.

It should not print, rotate, or recreate secrets.

### 4. Isolate mutable work

If the task changes code, the default should be:

- isolated branch
- isolated worktree
- isolated app slot when the stack needs one

### 5. Separate the loops

There are really two loops:

- the cheap inner loop that helps the agent move quickly
- the stricter final loop that proves the finished candidate

### 6. Keep browser proof and API proof separate

If the claim is about the UI, the browser must be part of the proof.

If the claim is only about an API contract, a browser run is unnecessary.

### 7. Make review role-separated

The useful shape is:

- maker builds
- reviewer finds issues
- simplifier trims unnecessary complexity
- verifier checks the final candidate

### 8. Keep loops idempotent

A recurring loop should do one fresh tick:

1. read saved state
2. read current state
3. compare
4. act only if allowed and necessary
5. save the new state
6. exit

### 9. Tie progress to proof

The progress update should reflect completed verified milestones, not elapsed time or guesswork.

### 10. Stop on purpose

Every skill needs a stop rule. Without a stop rule, the skill drifts.

## Why these public copies differ

My private skills include project paths, environment names, and tool wrappers.

The public skills are not simplified samples. They keep the operational parts
that make the real workflows useful:

- workflow shape
- inputs and routing
- authority and safety boundaries
- evidence and failure handling
- state, retry, and freshness rules where needed
- cleanup and stop conditions

Only the private adapter is removed: real repositories, ticket prefixes,
accounts, credentials, hosts, ports, and organization-specific commands.
