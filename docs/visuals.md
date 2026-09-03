# Visual System

The raster images in this repo were generated through the Google Gemini image API. The source screenshots used for composition stayed private because they contained identifying project and account details.

Mermaid handles process logic. Gemini handles the scene-setting images that make the workflow easier to understand at a glance.

## Art direction

The visual language stays consistent:

- dark graphite background
- electric blue and cyan for focus and active work
- amber for background work and attention
- terminal and macOS-inspired panels
- large readable labels
- abstract status bars instead of fake code
- no client names, repo names, usernames, emails, URLs, or credentials

## Assets

### Round-robin goals

![Round-robin goal visual](../assets/round-robin-goals.jpg)

Purpose: show the real concurrency model. Human focus rotates briefly between tasks. The goals keep running.

### cmux cockpit

![cmux cockpit](../assets/cmux-cockpit.jpg)

Purpose: explain the workspace and pane model. One project maps to one workspace. One task maps to one tab. Focus, context, proof, and long-running work stay visible together.

### cmux notifications

![cmux notifications](../assets/cmux-notifications.jpg)

Purpose: show why the operator does not need to stare at every session. Notifications bring them back only for a decision, completion, or failure.

### Terminal context hub

![Terminal context hub](../assets/terminal-context-hub.jpg)

Purpose: explain how tickets, docs, design, email, transcripts, and cloud systems can be pulled into the terminal through CLI, MCP, and APIs.

### Milestone update

![Milestone update](../assets/milestone-update.jpg)

Purpose: show proof-based progress arriving on WhatsApp through OpenClaw while long-running loops keep working on the laptop.

## Why use both images and diagrams

Images help with the cockpit idea, the notification pattern, and the feel of long-running work without babysitting.

Mermaid helps with exact process order, loops, and tool-selection logic.

## Prompt principles

Every image prompt followed the same rules:

```text
State the one concept the image must teach.
Name the intended placement and aspect ratio.
Keep the art direction consistent.
Allow only a few large labels.
Forbid copied names, URLs, and code.
Prefer abstract status bars over filler text.
Inspect every generation at full size.
Fix one defect at a time.
```

## Why the real screenshots are not published

Real workspaces leak too much:

- project names
- branch names
- account identities
- internal URLs
- terminal output
- sometimes secrets

Redrawing the concepts keeps the article useful without publishing the private workspace itself.
