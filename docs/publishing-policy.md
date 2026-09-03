# Publishing Policy

This repo is public. The local workflows it was derived from are not.

The public version is a clean rewrite, not a raw dump.

## Never Publish

- API keys, tokens, passwords, cookies, private keys, or secret payloads
- personal phone numbers or private email addresses
- company or customer names when the goal is a generic workflow
- private domains, clusters, namespaces, repositories, or ticket keys
- personal absolute filesystem paths
- kubeconfig contents or environment files
- production mutation commands copied from private operator playbooks
- raw screenshots of live workspaces
- raw meeting transcripts or sensitive email content

## Public Skill Rules

Every public skill should:

- solve a repeated cross-project problem
- describe the workflow in generic terms
- separate read-only discovery from mutation
- explain what proof is required
- explain what should make the workflow stop
- fail visibly when proof or access is missing

## Images

Published visuals must be fully redrawn and inspected.

Reject any image that contains:

- copied names
- copied URLs
- copied code
- account details
- secrets
- unreadable or misleading generated text

Exact logic should live in Mermaid where possible.

## Private Adapter Boundary

Public core:

```text
Use the configured cloud CLI to inspect the deployment.
```

Private adapter:

```text
Cloud: <real provider>
Account: <real identity>
Project: <real target>
```

The public repo may define the pattern. It must not publish the filled values.

## Pre-Publish Checklist

Before pushing:

1. inspect the staged diff
2. search for names, emails, phone numbers, domains, and obvious secret patterns
3. validate every skill
4. inspect the images at full size
5. render-check Mermaid blocks
6. confirm no caches, local state, or environment files are staged
7. run one independent review pass
