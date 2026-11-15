---
name: one-go-bg
description: Background one-go. A fresh background agent implements, reviews, commits, opens a pull request, and merges, while this session stays free.
disable-model-invocation: true
argument-hint: The task to land
---

# one-go-bg

The same landing as `one-go`, run by one background agent.
This session stays free for talk.
Read `docs/agents/delegation.md`, `docs/agents/git.md`, and `docs/agents/testing.md` before you start it.
Starting this skill is the Owner's permission for the background agent and the reviewers it starts, and for nothing more.

## Process

1. **Need a task.**
   When the Owner named no work, ask, and start nothing.
2. **Model.**
   When the Owner named a model, pass it to the agent tool.
   When the tool refuses the name, stop and say that it is not configured.
3. **Hand off.**
   Record the exact baseline commit, the target branch, the task, any uncommitted changes the task needs, and any review or test-deferral instruction.
   Start one fresh general-purpose agent in the background with the agent's own tool, from `adapters/agents.md` in the Kit or the Project's delegation page.
   Give it a 3 to 5 word label and the prompt below.
   Do not poll it.
4. **Stay free.**
   Tell the Owner that the agent is running, then stop.
   Do not edit its checkout or its branch.
5. **Report.**
   When it finishes, tell the Owner the result in one or two lines.
   For a requested human review, show the full review handoff and keep its checkout until the Owner approves.
   Further fixes go to a fresh agent with an exact handoff, never to a resumed one.

## Agent prompt

```text
You are the background owner of this task: you implement it and publish it.

1. Get your own isolated checkout, as docs/agents/delegation.md describes.
   Work only there. Check out the exact baseline below, and apply the supplied changes.
2. Read docs/agents/delegation.md, docs/agents/git.md, and docs/agents/testing.md there.
3. Read the Project's one-go skill and follow it to the end.
   Use a unique working branch; the foreground may hold the target branch.
4. Do not overwrite remote changes. When the target moved, merge it in, recheck, and review again.
5. Stop for a requested human review, a required approval, or a concrete blocker.

Task:
<the Owner's task>

Handoff:
<baseline commit; target branch; uncommitted changes; spec; earlier findings;
permissions; test deferral; human review checkpoint>

Report: branch, commits, pull request URL, merge result, check results,
and the checkout you kept, if any.
```
