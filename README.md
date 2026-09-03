# The Terminal-First Goal Loop

## A simple way to use AI across several software projects without losing focus

I often work on several unrelated software projects at the same time. The hard part is not opening more terminals or running more AI agents. The hard part is knowing:

- what deserves my attention;
- what an agent can do alone;
- how each task will be tested;
- when the work is truly done.

My workflow is built around four ideas:

1. Each project has its own workspace.
2. Each task has a clear goal and a way to prove it works.
3. I make the important product and risk decisions. Agents handle repeatable work.
4. Work is done only when it passes the right test in the right environment.

The tools may change. The method should not.

## 1. Start with a few simple rules

### Start from the user's experience

For a new product or a large new feature, I begin with the use cases and user journey. I do not start with the database.

If a design already exists, I use it. If not, I may ask Claude or another model to create a simple HTML prototype. The prototype helps me see the flow before I invest in production code.

For an existing product, I read the real code first. Tickets, documents, designs, emails, and meeting notes explain the intent. The code and the running product show the truth.

### Decide how to test before building

Before the agent writes code, I define:

- what the user should be able to do;
- what must not break;
- the smallest test that can fail;
- the final proof needed before I call the task done;
- which decisions still need me.

The implementation plan and the test plan are one plan.

### Use the cheapest useful test

I do not rebuild and redeploy the whole system after every edit. I start with the smallest test that can prove the current change. I use hot reload when possible. I run expensive end-to-end tests only after the smaller checks pass.

### Separate the maker from the reviewer

The agent that writes the code should not be the only agent that judges it. Important work gets an independent review of the exact version I plan to ship.

### Prefer less code

Before adding code, I ask:

1. Does this need to exist?
2. Does the codebase already solve it?
3. Can the standard library or platform handle it?
4. Does an installed dependency already cover it?

I add a new abstraction or dependency only when the simpler options fail.

### Keep important authority human

Agents can inspect, plan, code, test, and prepare actions inside an agreed scope. Shared or risky actions need the authority defined for the task. Examples include publishing comments, creating tickets, merging, deploying, deleting, purchasing, or changing scope.

## 2. Use cmux as a project cockpit

I use [cmux](https://cmux.com/) to keep projects separate. Each project gets a named workspace in the sidebar. Each task gets a named tab inside that workspace.

The workspace already tells me the project, so the tab name describes the task:

```text
<type>/<short-outcome> [state]
```

Examples:

```text
feature/export-history [build]
bug/session-loop [repro]
review/pr-142 [verify]
ops/release-candidate [ci]
```

I rename the tab when the state changes. A stale name creates false context.

### My pane layout

I use two panes by default. I split into four only when I need to see context and proof at the same time.

When I use four panes:

- **Bottom left: Focus.** The task I am actively driving.
- **Top left: Goal.** The long-running agent, plan, or progress log.
- **Bottom right: Proof.** Tests, reviews, browser checks, CI, or runtime logs.
- **Top right: Context.** Figma, Jira, Confluence, designs, or dashboards.

Keeping the same layout across projects gives me muscle memory.

### Limit work in progress

A large screen can show many sessions, but my attention is still limited. My default limits are:

- one project gets my direct attention at a time;
- one task is driven by me;
- no more than two agents change code at the same time;
- each project has at most three task tabs: `active`, `next`, and `parked`;
- read-only watchers are allowed only when they stay silent if nothing changed.

Anything else goes into a backlog, not another pane.

I switch projects at a clear milestone: the plan is ready, the code is waiting for CI, the agent needs a decision, or a tested version is ready for review.

## 3. Put every task into one of six lanes

I use six main task types:

1. **Discovery and design:** a new product, major journey, or unclear problem.
2. **Feature:** new behavior in an existing product.
3. **Bug fix:** current behavior breaks an expected rule.
4. **Review:** checking someone else's proposed change.
5. **Operations and release:** infrastructure, migrations, deployment, recovery, or production checks.
6. **Knowledge and communication:** tickets, documents, decisions, reports, and handoffs.

I add a label such as `ui`, `backend`, `data`, `security`, `integration`, or `infrastructure` when it helps.

UI improvement is usually a feature or bug fix with a `ui` label. Investigation is a step that can happen in any lane.

### A task is not the same as a pull request

I may group several tickets into one goal when they share the same user journey, code area, or test path. One ticket may need several pull requests when different repositories, owners, or release steps are involved.

I split pull requests by what can be reviewed, deployed, rolled back, and tested safely. I do not split them by ticket count alone.

## 4. Bring context to the terminal

I try to work through machine-friendly tools instead of repeatedly opening web pages. This makes the work easier to repeat, automate, and verify.

Common sources include:

- Jira for requirements and ticket history;
- Confluence for decisions and architecture;
- Figma for intended screens and journeys;
- source code and git history for real behavior;
- Gmail for narrowly scoped business context;
- Fireflies for approved meeting transcripts;
- Cloudflare, `gcloud`, `doctl`, and other CLIs for runtime facts.

I also use [Wispr Flow](https://wisprflow.ai/) to speak instead of typing. A voice dump is only raw material. The agent must still organize it, remove repetition, and point out missing or conflicting decisions.

### Build a small context pack

Before planning, I collect only what the task needs:

```text
problem
affected user
relevant tickets and decisions
current behavior from code or runtime
design references
constraints and non-goals
open decisions
```

More context is not always better. A large dump can hide the important facts and increase cost.

### Handle email and meeting transcripts carefully

Email access should be read-only by default. I define the account, search or labels, time range, and what may be saved.

Meeting transcription depends on company policy, local law, and participant notice or consent. The raw transcript is source material, not the final document. I keep a short summary with the date, decisions, actions, and source. I do not publish raw transcripts by default.

## 5. Give agents access without exposing secrets

Every organization should have one read-only access checker. It should report:

- whether access is available, partial, or unavailable;
- where the credential comes from, without showing its value;
- which account, project, tenant, or cluster is active;
- which CLI, MCP, or API to use;
- a safe command that proves access;
- the exact blocker when access fails.

Finding a credential and changing a credential are different tasks.

I never trust the shell's default cloud account, Kubernetes context, or project. Every command names the intended target. Skills point to the secret authority; they never contain the secret itself.

## 6. Turn the task into a goal

I call this step **goal engineering**. The goal is not to write a huge prompt. The goal is to make the result clear enough that an agent can work for hours without inventing scope or stopping too early.

Every important task starts with a small Focus Session note:

```markdown
# Focus Session

Outcome:
Task type and surface:
User-visible acceptance criteria:
Scope:
Non-goals:
Exact version being tested:
Test ladder:
Human decisions:
Allowed external actions:
Stop conditions:
Next handoff:
```

A small task may fit in one paragraph. A risky or long-running task may need a full plan, a list of affected areas, and a table that maps each requirement to its proof.

### A goal is ready when

- “done” has an observable meaning;
- the affected areas are listed;
- every acceptance rule has a test;
- tests are ordered from cheap to expensive;
- outside dependencies and human decisions are named;
- one main session owns the goal;
- child agents have clear, limited work;
- review and fix loops have a maximum number of rounds;
- the tested version can be identified by commit, build, image, or file hash.

If the agent cannot name the smallest test that can fail, the goal is not ready to run alone.

## 7. Feature development

### Design and plan

1. Gather the relevant Jira, Confluence, Figma, email, and code context.
2. Describe the user journey in plain language.
3. Use the real design, or create a disposable HTML prototype.
4. Review the happy path, errors, empty states, permissions, data ownership, and database impact.
5. Explain the implementation idea to the agent.
6. Let the agent inspect the code and challenge the idea.
7. Write the acceptance rules and tests.
8. Ask a second model to review a complex plan.
9. Choose one useful, testable slice to build.

For frontend work, I use [Taste](https://github.com/leonxlnx/taste-skill) or [Impeccable](https://github.com/pbakaus/impeccable) to improve visual judgment, hierarchy, accessibility, responsiveness, and interaction quality. These skills improve the design. They do not replace requirements or browser testing.

### Build and test

1. Create an isolated worktree and runtime slot.
2. Confirm the current tests are green.
3. Write focused unit tests for non-trivial logic.
4. Write lasting integration or API tests for important contracts.
5. Implement the smallest complete change.
6. Use hot reload and the smallest failing test while coding.
7. Run wider tests only after the smaller checks pass.
8. If users see the change, run the real flow with [agent-browser](https://github.com/vercel-labs/agent-browser) and save screenshots.

## 8. Bug fixing

Bug fixes follow a stricter order:

1. **Reproduce the problem.** A bug report is a claim until I can see it.
2. **Find the root cause.** Trace the real caller chain instead of patching the visible symptom.
3. **Make the decision.** Show what is wrong, who it affects, the possible fixes, and the recommended fix. Sometimes the report is not a bug, or the correct behavior is unclear.
4. **Write a failing test.** Prefer an integration, API, or browser test that represents the real report. See it fail before changing the code.
5. **Fix the root cause.** Fix the shared boundary once instead of adding guards to every caller.
6. **Run the same test.** See it pass locally or in the isolated slot.
7. **Review and simplify.** Use the same final quality gate as feature work.
8. **Ship the exact tested version.**
9. **Run the same test in the affected environment.** A bug found in a demo or live system is not done until the test passes there on the shipped code.

If I cannot reproduce the bug, I stop and say so. I do not create a fix for behavior I cannot see.

## 9. Isolate work and keep infrastructure warm

Each coding task gets its own git worktree. This gives each agent a separate branch, folder, and clean diff without constant branch switching.

I do not start a full infrastructure stack for every worktree. I keep expensive shared services running once. Each worktree gets a numbered application slot with predictable ports and isolated app processes.

I use two slot types:

- **Hot slot:** attached development processes for the task I am editing.
- **Passive slot:** background services for another agent or comparison test.

Every slot needs simple commands to start, list, test, stop, and release it. Stopping a slot must not stop the shared infrastructure. Cleanup is part of finishing the task.

## 10. Climb the test ladder

I use the lowest level that can prove the current claim:

1. formatting, lint, types, and static checks;
2. unit test;
3. component or service test;
4. integration or real API test;
5. local request with hot reload;
6. isolated slot or sandbox;
7. real browser or user flow;
8. CI on the exact commit;
9. deployed runtime tied to that commit or image;
10. final end-to-end test in the target environment.

A passing level allows the agent to move up. A failure sends it back to the cheapest level that can reproduce that failure. It does not force unrelated tests to run again.

For UI work, a screenshot needs context: the action, screen size, tested version, and expected result. An API response does not prove the UI works. A successful deployment does not prove the user journey works.

## 11. Review, simplify, and verify

My final quality gate is:

```text
tests green
-> simplify only the changed files
-> run the affected tests again
-> independent review of the exact version
-> fix confirmed problems in a small, coherent batch
-> run affected tests
-> fresh final review or verifier
-> commit or push only when clean
```

In Claude Code, I use Anthropic's [PR Review Toolkit](https://github.com/anthropics/claude-plugins-official/blob/main/plugins/pr-review-toolkit/README.md) for focused reviews of code, tests, types, comments, and silent failures. I use Anthropic's [code-simplifier](https://github.com/anthropics/claude-plugins-official/blob/main/plugins/code-simplifier/agents/code-simplifier.md) for behavior-preserving cleanup.

I also use [Superpowers](https://github.com/obra/superpowers) for structured planning and development, and [Ponytail](https://github.com/DietrichGebert/ponytail) to push the implementation toward the smallest correct solution.

The reviewer finds possible problems. A separate verifier may confirm or reject those findings, but it must not invent new findings during that check.

The amount of review depends on risk:

- a tiny, reversible change may need only its focused test;
- normal feature work gets one independent review;
- security, permissions, payments, migrations, destructive work, and production releases need stronger review from fresh context.

## 12. Let agents run useful loops

AI should keep going when the next action is safe, expected, and inside scope. But every loop needs rules.

Each loop defines:

```text
when it starts
what one pass does
what state it saves
how it avoids doing the same action twice
what it may change
what needs a human
what it says when nothing changed
when it stops
maximum retries
```

### The loops I find useful

**Development loop**

```text
exact failure -> smallest test -> minimal fix -> hot reload -> same test passes -> next boundary
```

**Review and fix loop**

Review the exact version, group duplicate findings, fix the smallest coherent set, and rerun only the affected tests. Stop when clean, when no progress is made, or when the round limit is reached.

**Pull request babysitter**

Check only what changed since the last pass. Watch comments and CI. Verify replies against code, not against what the author says. Stay silent when nothing changed. Stop when the pull request closes, merges, or needs a real human decision.

**CI and deployment watcher**

Watch one job tied to one exact version. Stop on success, failure, cancellation, timeout, or a new version. Report the failed job and useful error, not every poll.

**Dependency wait loop**

Check one outside condition per pass. Do not re-plan the task while nothing changed. Resume from the saved step when it changes.

**Runtime monitor**

Batch health checks and compare them with the previous result. Notify only when something meaningful changes or when a scheduled report is due.

**Knowledge loop**

After an important meeting, decision, or incident, write a short note with the date, source, decision, and actions. Publishing the note still follows the external-action rule.

**Cleanup loop**

After a merge or cancelled task, find unused worktrees, branches, slots, browser sessions, port forwards, and test data. Recheck each target before cleanup. Never remove dirty or unclear state.

I do not create a loop for every task. A loop is useful only when the check repeats, changes are cheap to detect, actions can be repeated safely, and the stop condition is clear.

## 13. Choose models by the job

In my current setup, Codex usually handles implementation and orchestration. Claude Code often helps with design, plan review, or a second opinion. The important part is the separation of roles, not the product name.

I route work like this:

- **Fast, cheaper model:** file search, summaries, status checks, ticket drafts, and simple transformations.
- **Strong builder:** multi-file coding and long feature work.
- **Deep reasoner:** architecture, unclear bugs, security, permissions, money, migrations, and difficult plans.
- **Independent model:** plan review and final verification when a different perspective matters.
- **Design model or skill:** user experience, visual direction, copy, and interaction polish.

I reserve fast modes for work where latency matters. A long task can run at normal speed and report only at useful milestones.

I parallelize independent reading and analysis. I normally serialize code changes. Two agents change code at once only when they own separate areas and combining the work will be easy.

## 14. Report progress with proof

Percentages should come from finished steps, not time spent or intuition.

I set the steps before the task starts. A useful default is:

| Progress | Finished step |
| ---: | --- |
| 0% | Task captured but not scoped |
| 10% | Context and scope clear |
| 20% | Acceptance rules and test ladder written |
| 30% | Plan reviewed; worktree and environment ready |
| 50% | Main implementation complete |
| 65% | Focused tests and static checks pass |
| 75% | Integration, slot, sandbox, or browser proof passes |
| 85% | Simplification and independent review complete |
| 90% | Pull request and required CI pass |
| 95% | Exact version deployed to the target environment |
| 100% | Final proof, cleanup, and handoff complete |

For a bug fix, reproduction and a failing test must happen before implementation. If a task has no deployment, I remove those steps and recalculate the percentages.

Each update is short:

```text
<percent> | done: <step> | proof: <result> | next: <step> | blocked: <none or exact reason>
```

For long tasks, I send these updates through a channel such as WhatsApp using a local automation bridge. I send milestones and decisions, not raw terminal logs.

I never report 100% while a required test, cleanup step, or human decision is still open.

## 15. Keep useful memory, not entire conversations

I use four layers of memory:

1. **Raw input:** voice dumps, emails, transcripts, screenshots, and logs. Keep them private and delete them when the task or policy says to.
2. **Task memory:** goal, plan, decisions, current version, test results, blockers, and next step.
3. **Repository memory:** local rules, architecture, commands, and common mistakes stored close to the code.
4. **Long-term memory:** short lessons that can help future tasks, with the source and date last checked.

I save decisions and reusable proof, not whole chats. I never save secrets as memory. I mark guesses as guesses and recheck facts that may have changed.

When a task moves to another session or agent, the handoff includes:

```text
goal
exact version
proof already completed
open findings
current wait state
next smallest action
actions that are not allowed
```

The goal should survive a new session. The full conversation should not be needed to rebuild it.

## 16. Prefer CLI, MCP, and APIs over repeated UI work

Machine interfaces reduce friction. Agents can repeat commands, inspect errors, combine tools, and leave an audit trail.

For normal execution, I prefer:

1. an official CLI or API;
2. a scoped MCP tool when it gives a safer, clearer operation;
3. a small script around an authorized API;
4. browser-assisted login or request replay;
5. browser page automation;
6. manual UI only when no reliable machine path exists.

The browser has one exception: for a user-facing feature, the browser is the main proof that the experience works.

Cloud commands always name the intended account, project, region, tenant, or cluster. I check access and use dry runs before making changes. Commands must not print credentials.

I read and draft Jira tickets or Confluence pages through a CLI or MCP tool. The agent shows me the proposed external write, then performs it only when the task allows it. I use the web UI for visual checking, not as the default work surface.

### Use APIify to remove repeated browser work

[Apiify](https://github.com/sabirmgd/apiify-skills) turns an authorized browser workflow into a reusable script. The browser is used once for discovery or login. The final tool uses the cheapest reliable option:

```text
public API
-> private XHR or GraphQL request
-> authenticated browser request
-> page automation as a last resort
```

The result should be a readable script with clear inputs, structured output, safe authentication, a real test, and known risks.

API discovery must never be used to bypass access controls, paywalls, CAPTCHAs, company policy, or site rules.

## 17. Know what “done” means

A task is done when:

- the user or operator can achieve the intended outcome;
- every acceptance rule has current proof;
- focused and integration tests pass;
- user-facing behavior has browser or real-user proof;
- CI and the running system are tied to the exact tested version when needed;
- confirmed review findings are fixed;
- simplification did not change behavior;
- external actions were authorized;
- test data, sessions, slots, and worktrees are cleaned up;
- the handoff names any remaining risk honestly.

Local tests, a pushed branch, a merged pull request, a successful build, and a green deployment are useful milestones. None of them alone proves the user's outcome.

## What I would standardize next

The next improvement is not another tool. It is using the same small rules everywhere:

1. Use the Focus Session note for every important task.
2. Fix the cmux pane roles, tab names, and work-in-progress limits.
3. Use the six task lanes and proof-based percentages.
4. Give every loop saved state, a quiet path, an authority limit, and a stop rule.
5. Keep one main copy of each workflow and use small adapters for different AI tools.
6. Route work by role instead of hardcoding model versions unless a pin is truly needed.
7. Keep secrets and identifying details out of skills, prompts, logs, screenshots, and examples.

## Reference tools

- [cmux](https://cmux.com/) and its [source repository](https://github.com/manaflow-ai/cmux)
- [Codex](https://developers.openai.com/codex/)
- [Claude Code](https://docs.anthropic.com/en/docs/claude-code/overview)
- [Wispr Flow](https://wisprflow.ai/)
- [Fireflies security and privacy](https://fireflies.ai/security)
- [Fireflies consent and notification controls](https://guide.fireflies.ai/articles/3917896272-how-consent-notifications-work-on-the-fireflies-desktop-app)
- [agent-browser](https://github.com/vercel-labs/agent-browser)
- [Superpowers](https://github.com/obra/superpowers)
- [Ponytail](https://github.com/DietrichGebert/ponytail)
- [Anthropic PR Review Toolkit](https://github.com/anthropics/claude-plugins-official/blob/main/plugins/pr-review-toolkit/README.md)
- [Anthropic code-simplifier](https://github.com/anthropics/claude-plugins-official/blob/main/plugins/code-simplifier/agents/code-simplifier.md)
- [Taste](https://github.com/leonxlnx/taste-skill)
- [Impeccable](https://github.com/pbakaus/impeccable)
- [Apiify](https://github.com/sabirmgd/apiify-skills)

## The method in one sentence

> Capture the idea, gather the right context, define the goal and tests, isolate the work, use the cheapest useful feedback loop, verify from the user's side, review with a fresh mind, and let safe loops handle the repeatable work.
