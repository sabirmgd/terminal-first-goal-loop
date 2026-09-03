---
name: access-check
description: Locate and verify the access path for a system, credential, or operator surface without exposing secret values. Use when work is blocked on login, cloud access, API credentials, cluster context, CI secrets, or tool authority and the task needs read-only proof of what is available.
---

# Access Check

Use this skill to answer a simple question safely: do we actually have access,
and where does that access come from?

## Read-Only Rules

1. Stay read-only.
2. Never print secret values, reversible encodings, cookies, tokens, kubeconfig
   bodies, or password-manager contents.
3. Report the authority, not the secret itself.
4. Verify identity and target explicitly. Do not trust the current shell context.
5. Separate source authority from runtime delivery.

## Workflow

1. Name the exact surface.
2. Identify the authority in this order when possible:
   - secret manager or vault
   - platform credential store
   - dedicated local config
   - CI secret inventory
   - repo handoff file only when it is clearly meant for local access
3. Run the smallest read-only proof.
4. Report one of three states:
   - `available`
   - `partial`
   - `unavailable`

`available` means the required read or write capability was actually proved. A public page, an installed CLI, a secret name, or a working login to a different account is not enough.

## Safe Proof Examples

- list authenticated identities without tokens;
- run a `whoami` or account command;
- list project, cluster, repository, or secret names;
- inspect file permissions and key names without values;
- call a read-only health or identity endpoint;
- confirm a browser session reaches the required signed-in surface.

If a command reveals a secret by default, replace it with a metadata or presence check.

## Failure Routing

- CLI exists but authentication is missing: report the missing login or credential source.
- Authentication works but the target fails: report the account, project, region, tenant, or permission mismatch.
- Secret authority is healthy but runtime lacks the value: report a delivery problem, not a missing secret.
- A public URL works but admin access is unknown: report `partial`.
- A local handoff exists but freshness is unknown: report the staleness risk.

## Output Contract

```text
ACCESS CHECK | surface=<name> | status=<available|partial|unavailable>
SOURCE | type=<vault|ci|config|browser|other> | location=<path-or-name>
IDENTITY | account=<non-sensitive identity or unknown> | target=<resource>
PROOF | command=<read-only check> | result=<short result>
BLOCKER | none or exact missing piece
```

## Stop Conditions

Stop when the access question is answered with enough proof for the task.

Stop immediately if the next step would print, decode, rotate, copy, create, or deploy a secret. Hand that work to a private access-repair or secret-management workflow with separate authority.
