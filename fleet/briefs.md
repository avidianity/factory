# Briefs

A Brief is the message that starts a Worker.
It is the only context the Worker has, so it must stand alone.

## What a Brief contains

```text
Name: w-<short-task-name>
Project: <repository and checkout path>
Task: <the outcome, in one or two sentences>
Source: <Ticket link, or the Owner's words, quoted>
Expected Tier: <0-4, from the Ticket>
Baseline: <branch and commit>
Scope: <allowed paths; forbidden paths>
Done means: <merged pull request | report | draft for the Owner>
Permissions: <what the Owner allowed: merge, deploy, external writes>
Decisions already made: <relevant entries from the decision log>
Ask before: <anything the Worker must not decide alone>
Report to: the Coordinator, with <format>
```

## Rules

- **Name every Worker** with the `w-` prefix and a short task name, such as `w-login-timeout`.
  A clear name lets the Coordinator and the Owner see what each session does.
- **One task per Worker.**
  A new task gets a new Worker, even when an idle one exists.
- **Forward the Owner's words.**
  When the Owner's message is the spec, quote it exactly; do not paraphrase it.
- **Name the Decisions.**
  Copy each relevant Decision from the decision log, with its date.
  Do not assume the Worker can find it.
- **Literal samples.**
  Show a real command, payload, or error line instead of a description.
- **Frontier only.**
  Brief a Worker only for a Ticket whose blockers are all closed.
  When a Worker's merge closes a blocker, brief the Tickets it unblocks.
- **Sharpen from Tier 2.**
  For a Ticket of Expected Tier 2 or higher, run `sharpen` on the task before you send the Brief.
  Keep the Ticket's scope and acceptance criteria as written.
  Send each choice that `sharpen` raises and no experiment can settle to the Owner as a Decision.
- **No secrets in a Brief.**
  Name where the Worker finds a credential, never the value.

## Reports

A Worker's report to the Coordinator has:

- the result: done, blocked, or waiting for a Decision;
- links: pull requests, issues, commits;
- check results, with failures classed as introduced, pre-existing, or environmental;
- the question, when one is open, with the Worker's recommendation.

The Coordinator relays a short version to the Owner.
It never relays a Worker's guess as a fact.

## Handoffs between Projects

When a Worker finishes work that needs a change in another Project, the Coordinator sends a handoff to a Worker of that Project.
The handoff contains the issue, the merged pull request, the contract with a literal sample, any released version, and exactly what the other Project must do.
