# Visual System

The illustrations in this repository were generated through the Google Gemini API with `gemini-3.1-flash-image`. The original workspace screenshots were used only as private composition references. They are not published because they contain identifying project and account details.

## Art direction

All images use the same visual language:

- dark graphite background;
- electric blue and cyan for active human focus;
- amber for long-running work and attention;
- native macOS and terminal-inspired panels;
- large readable labels;
- abstract status bars instead of fake code;
- no client names, repositories, usernames, emails, URLs, credentials, or copied logos.

Generated images explain a concept or working scene. Mermaid handles exact process logic because it stays readable, editable, and accurate.

## Assets

### Round-robin goals

![Round-robin goal visual](../assets/round-robin-goals.jpg)

Purpose: show the real concurrency model. The human spends a short time shaping each task, while every launched goal keeps running.

Alt text: Four task stations each have a short five-to-ten-minute shaping step and a much longer active goal track. Human attention moves between tasks, while agent work continues and returns at decision or proof checkpoints.

Visual QA score: 98/100.

### cmux cockpit

![cmux cockpit](../assets/cmux-cockpit.jpg)

Purpose: explain the mapping from project to workspace, task to tab, and the roles of focus, goal, context, and proof panes.

Alt text: An anonymous terminal workspace with four project workspaces in a sidebar and four panes for a long-running goal, context and design, human focus, and proof and tests.

Visual QA score: 94/100.

### cmux notifications

![cmux notifications](../assets/cmux-notifications.jpg)

Purpose: show why long-running agents do not need constant watching. Notifications bring the human back for a decision, completion, or failure.

Alt text: A generic notification panel shows one task needing a decision, one completed goal, and one failed CI check over an anonymous terminal workspace.

Visual QA score: 92/100.

### Terminal context hub

![Terminal context hub](../assets/terminal-context-hub.jpg)

Purpose: explain how Jira, Confluence, Figma, Gmail, Fireflies, and cloud tools reach the task through CLI, MCP, and APIs. The browser remains available for discovery and user-facing proof.

Alt text: Six optional context sources connect to a central terminal using CLI, MCP, and API paths, with a separate browser below for discovery and proof.

Visual QA score: 97/100.

### Milestone update

![Milestone update](../assets/milestone-update.jpg)

Purpose: show proof-based progress arriving on a phone while several agents continue working on an awake laptop.

Alt text: A laptop with several active agent progress rings sends a message to a phone showing 65 percent complete, passed integration tests, proof count, next browser journey, and no blocker.

Visual QA score: 93/100.

## Prompt principles

The exact scene changed for each asset, but every prompt followed the same rules:

```text
State the one idea the image must teach.
Name the intended use and aspect ratio.
Reuse one consistent art direction.
List the exact labels that may appear.
Forbid copied names, URLs, code, credentials, and logos.
Ask for abstract status bars instead of filler text.
Inspect the result at full resolution.
Change one visual defect at a time.
```

Gemini can render useful diagrams, but generated text still needs inspection. Several early drafts had duplicated numbers, misspelled labels, or copied background fragments. Those drafts were rejected and regenerated before publication.

## Why not publish the source screenshots?

A real development cockpit is full of information that should not become article decoration: project names, branches, account identities, internal URLs, terminal output, and sometimes credentials. Redrawing the idea keeps the article useful without exposing the working environment.
