---
name: bug-repro
description: Turn a bug claim into one standalone same-run repro bundle with a strict yes, no, or inconclusive answer. Use when a report, screenshot, error snippet, QA note, or support message may describe a bug and the first job is to prove or disprove it without starting diagnosis or implementation.
---

# Bug Repro

This skill has one job:

```text
claim -> one reproducible proof -> handoff -> stop
```

It does not fix the bug. It does not choose a branch. It does not create a PR.

## What this skill owns

- restating the claim
- turning the claim into expected behavior
- choosing the narrowest proof surface
- building a standalone repro bundle
- running the repro once
- returning a strict outcome

## What this skill does not own

- root-cause diagnosis
- code changes
- worktree creation
- ticket updates
- release claims

## Classify the claim first

Pick one primary surface:

- API or transport
- integration or job flow
- browser journey
- runtime or environment behavior

Do not mix several surfaces unless one really cannot prove the claim alone.

Examples:

- wrong status code: API
- background job never updates state: integration or runtime
- page renders the wrong thing: browser
- role sees data it should not: API first, browser if the visible page is the claim

## Translate the report into an expected contract

A bug report usually describes the bad result. The repro needs the expected result.

Examples:

- "this times out" becomes "this request should return within the expected bound"
- "this page is blank" becomes "this page should render the expected loaded state"
- "manager cannot save" becomes "manager role should be allowed to save this form"

Do not write a repro that merely confirms the bad behavior happened. Write a repro that checks whether the intended behavior holds.

## Build a standalone repro bundle

Create one bundle outside the product code path so it can be rerun without reopening the whole investigation:

```text
.omx/repro-runs/<YYYYMMDD-HHMMSS>-<slug>/
  manifest.md
  repro.sh
  evidence/
```

`manifest.md` should record:

- the original claim
- the expected contract
- the chosen surface
- inputs or role assumptions
- the exact command or steps inside `repro.sh`
- where evidence will be written

`repro.sh` should do one thing only: execute the exact repro and write same-run evidence.

If shell is the wrong surface, use the lightest equivalent, but keep the same bundle contract. The output must remain rerunnable and self-contained.

## Keep the run narrow

Rules:

1. Use one target surface.
2. Use one target role if the bug is role-specific.
3. Use one target environment.
4. Use one same-run evidence set.
5. Do not manually poke around before running the real repro unless access or setup must be checked first.

If a role-specific claim exists, record the role in the bundle. If the role cannot be proven because access is missing, return `inconclusive`, not `not reproduced`.

## Run once

Run the repro once and classify the result.

Allowed outcomes:

- `yes`: the expected contract failed, the bug is reproduced
- `no`: the expected contract held, the bug is not reproduced
- `inconclusive`: the repro could not answer honestly because of setup, access, transport, or environment failure

Do not run a second ad hoc test just to produce a cleaner answer. The bundle should contain the proof from the real run.

## When a surface probe is allowed

If the run is inconclusive because the environment or transport might be down, one narrow follow-up probe is allowed:

- one health or identity check for the same surface
- one authentication check for the same role or environment

Do not escalate into a full environment audit. The point of the probe is only to explain why the repro was inconclusive.

## Safety

- keep the default repro read-only
- if the claim inherently requires a state-changing repro, only do it with explicit authority and only against a safe non-production target
- do not print secrets, tokens, cookies, or raw sensitive payloads into the bundle
- redact or cap evidence when the response body is large or sensitive
- inspect screenshots before sharing them

## Handoff contract

Return exactly this shape:

```text
Reproduced: yes | no | inconclusive
Surface:    <surface>
Bundle:     <path>
Script:     <path>
Evidence:   <path>
Observed:   <what the run actually showed>
Expected:   <the pinned intended behavior>
Rerun:      <path>
```

You may add one short blocker line only when the result is inconclusive:

```text
Blocker: <exact reason>
```

Do not add:

- root-cause guesses
- fix options
- branch names
- PR ideas
- release verdicts

Those belong to the next lane.

## Stop conditions

Stop immediately when one of these is true:

- the bundle produced a clear yes
- the bundle produced a clear no
- the bundle is honestly inconclusive and the blocker is identified

If you find yourself diagnosing the bug or editing code, you are already in the wrong skill.
