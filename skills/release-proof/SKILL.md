---
name: release-proof
description: Prove that a candidate is truly ready or live by binding source, CI, artifact, runtime, and consumer evidence to the same exact version. Use when a feature, fix, preview, sandbox, or release should not be trusted from a green pipeline alone.
---

# Release Proof

Use this skill whenever a green pipeline is not enough.

## One Candidate, Five Layers

Every verdict must bind to one exact candidate identity such as a commit SHA,
image digest, artifact version, or deployment revision.

Prove these layers in order:

1. `source`
2. `ci`
3. `artifact`
4. `runtime`
5. `consumer`

If any layer is missing or mismatched, the verdict is not `pass`.

## Rules

1. Runtime proof must name the exact environment.
2. For UI claims, browser proof is mandatory.
3. For API-only claims, use the real client path or exact request shape.
4. A healthy runtime is not enough if it is serving the wrong candidate.
5. A green CI run is not enough if runtime or consumer proof is missing.
6. This skill proves state only. It does not authorize merge, deploy, retry, or rollback.

## Workflow

1. Freeze the candidate identity and the environment being judged.
2. Prove the source branch or commit is the intended candidate.
3. Collect the required CI runs for that exact candidate.
4. Prove the built image, package, or artifact was produced from it.
5. Prove the target runtime is ready and serving that artifact.
6. Run the real browser, API, CLI, or consumer journey.
7. Record every missing or mismatched layer as `blocked` or `fail`.

Do not judge a rollout while it is still converging. Wait for the terminal state, then re-read the live artifact and traffic identity.

## Common False Greens

- CI passed for one commit, but another commit was deployed.
- The expected artifact exists, but traffic still points to the old revision.
- The service is healthy, but required migrations or runtime configuration are missing.
- The backend endpoint works, but the browser journey is broken.
- A deployment workflow succeeded, but authenticated smoke tests were skipped.

## Output Contract

```text
RELEASE PROOF | candidate=<id> | verdict=<pass|blocked|fail>
SOURCE | <proof>
CI | <proof and result>
ARTIFACT | <proof and result>
RUNTIME | env=<name> | <proof and result>
CONSUMER | <browser, api, or client proof>
NEXT | <none or exact remaining gate>
```

## Stop Conditions

Stop with `pass` only when all required layers point to the same candidate and the consumer proof passes. Stop with `blocked` or `fail` as soon as an unresolved layer prevents a truthful verdict.
