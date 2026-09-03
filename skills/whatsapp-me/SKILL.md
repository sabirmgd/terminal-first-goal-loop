---
name: whatsapp-me
description: Send proof-based text or media updates through OpenClaw to a configured WhatsApp target with safe reply routing back to the right task, session, or worktree. Use when the user explicitly wants a WhatsApp delivery or when a workflow already has an approved OpenClaw notification route.
---

# WhatsApp Me

Use this skill to deliver a real update through OpenClaw, not to draft a
message that may or may not be sent.

## Why This Exists

If several tasks are running at once, a phone update is useful only when it can
do three things:

- say what actually changed
- include proof, not vague progress
- route replies back to the right task

If any of those fail, the send should fail closed.

## Required Configuration

This skill uses configured state only. Do not hardcode a phone number, media
directory, database path, or local user path.

Before sending, confirm:

- `openclaw` is installed and reachable
- the WhatsApp target is configured in env or OpenClaw config
- any staging path comes from config or platform defaults
- reply-routing storage is configured if replies are expected

If the target is not configured, stop. Do not guess.

## What To Send

Good sends:

- milestone update with proof
- screenshot with a short caption
- report or file with one-sentence context
- final handoff with task, proof, and next action

Bad sends:

- raw terminal logs
- repeated "still working" messages
- guessed percentages
- a reply that cannot route back to the right task

## Proof-Based Updates

Prefer this message shape:

```text
<percent or milestone>
Done: <what actually passed>
Proof: <test count, screenshot, CI result, revision, or other receipt>
Next: <next action>
Blocked: <none or exact blocker>
```

If the percentage is not honest, omit it.

## Delivery Workflow

1. Build the message from verified state.
2. Confirm the target and route are configured.
3. If sending media, confirm the file exists and is safe to send.
4. Send through OpenClaw.
5. Capture the native outbound message id or ids.
6. Store the routing record for replies:
   - outbound message id
   - task label
   - session id
   - thread id if available
   - worktree or repo hint
   - progress value if one was sent
7. Report success only if OpenClaw accepted the send and routing state was
   stored when required.

## Reply Routing

Replies should route by stored message id or trusted routing token, not by free
text pattern matching.

Safe rules:

- prefer a stored outbound message id
- prefer a trusted feedback token over a raw user-supplied id
- keep task, session, and worktree metadata with the outbound record
- reject expired, unknown, or mismatched routing tokens
- reject a reply if the required quoted-message id is missing

Never build shell commands from feedback text. Feedback is data, not code.

## Media

Media is allowed when it adds value:

- screenshot
- image
- short clip
- report file

Before sending media:

- verify the file exists
- respect transport size limits
- stage it through the configured OpenClaw path when required
- keep the caption short and proof-based

If media staging fails, the send fails.

## Fail-Closed Rules

Do not claim success when any of these are missing:

- configured WhatsApp target
- successful OpenClaw response
- native outbound message id for a routed conversation
- routing record for reply-aware sends
- quoted reply target for a reply send

If a routed reply cannot be sent as a real native reply, stop. Do not silently
fall back to a new unrelated message bubble.

## Output Contract

Successful send:

```text
WHATSAPP ME | state=sent
TARGET | configured=yes
PROOF | kind=<text|media> | receipt=<message-id-or-count>
ROUTE | task=<label> | session=<id-or-unknown> | worktree=<hint-or-unknown>
```

Failed send:

```text
WHATSAPP ME | state=blocked
BLOCKER | <missing target, missing route, media failure, OpenClaw failure, or reply mismatch>
NEXT SAFE STEP | <exact fix>
```

## Final Rule

This skill is for real delivery. If configuration, proof, or routing is not
good enough, stop and say exactly why.
