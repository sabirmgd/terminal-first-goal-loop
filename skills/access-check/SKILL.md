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

## Output Contract

```text
ACCESS CHECK | surface=<name> | status=<available|partial|unavailable>
SOURCE | type=<vault|ci|config|browser|other> | location=<path-or-name>
IDENTITY | account=<non-sensitive identity or unknown> | target=<resource>
PROOF | command=<read-only check> | result=<short result>
BLOCKER | none or exact missing piece
```
