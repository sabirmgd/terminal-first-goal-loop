---
name: docs-manager
description: Retrieve, draft, update, and publish docs in Confluence or a similar wiki from the terminal, with source grounding, version checks, and explicit write previews.
---

# Docs Manager

Use this when architecture notes, plans, handoffs, or operating docs should be read or written without leaving the terminal.

## Default mode

Read-only by default.

Safe default actions:

- fetch one page
- search pages
- compare page versions
- draft a new page
- draft an update to an existing page

Publishing or editing a live page needs explicit authority.

## Ground the document first

Before drafting or updating a page, ground it in actual sources:

- code
- tickets
- approved decisions
- test or runtime evidence
- existing docs

If the source evidence is weak, say so before writing polished prose.

## Read workflow

Return:

```text
System:
Space or section:
Page:
Current version:
Last updated:
Key points:
Open conflicts or stale spots:
Evidence:
```

Do not paste the whole page unless the task really needs it.

## Draft workflow

For a new page or update, prepare a preview with:

```text
Action:
Target:
Title:
Summary:
Sections:
Grounding:
Version base:
Exact write command or payload:
```

The preview should be ready for approval or light editing before any live publish.

## Publish workflow

Once authority is explicit:

1. resolve the exact page or parent
2. re-check the current page version
3. publish the draft against that version
4. read back the saved result

If the page changed underneath you, stop and show the version conflict. Do not overwrite it blindly.

## Rules

1. Keep architecture and status docs grounded in real code and evidence.
2. Do not silently merge conflicting edits.
3. Distinguish draft text from published text.
4. Prefer short, useful pages over giant generic summaries.
5. If the task is really a status report, use `status-report` instead of burying activity in prose.

## Stop rules

Stop when:

- the page read is complete
- the draft preview is ready
- the authorized publish is verified
- a version or permission conflict blocks safe publication
