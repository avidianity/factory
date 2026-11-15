---
name: one-go-parallel
description: Land several tasks with parallel agents. Each agent implements one task in its own checkout; this session integrates, publishes, and merges every pull request.
disable-model-invocation: true
argument-hint: Tasks or issue numbers, and the target branch if not the default
---

# one-go-parallel

This session is the coordinator: it plans, starts agents, integrates, publishes, and reports.
Each task goes to one fresh general-purpose agent that implements, tests, reviews, and commits in its own isolated checkout.
After the task list is clear, the next time the Owner hears from you is the final report, unless they asked for a review.

Read `docs/agents/delegation.md`, `docs/agents/git.md`, and `docs/agents/testing.md` before you start any agent.
Starting this skill is the Owner's permission for these agents and for every commit, push, pull request, and merge into the target branch.
It permits nothing else, such as a deploy or a release.

## Inputs

- **Tasks**: free text, issue numbers, or a spec.
  When none is named, ask and stop.
- **Target branch**: the Project's integration branch, unless the Owner names another.
- **Human review**: none unless the Owner asks.
- **Test deferral**: none unless the Owner asks.
- **Model**: when the Owner names one, pass it to every agent.

## Process

1. **Plan.**
   Split the request into tasks, one pull request each.
   For each issue, read it and add the owner label first.
   A task that builds on another task's unmerged code, or edits the same files heavily, joins a stack, ordered bottom to top.
   Every other task is independent.
   Done when each task has a one-line scope, a branch name, and a place: independent or in a stack.
2. **Prepare.**
   Confirm that the isolated-checkout tool works.
   Fetch the target branch, and record its tip as the baseline commit.
   Run the known-red baseline once, and record the failing tests.
   Agents reuse this list instead of running their own baseline.
3. **Start agents.**
   Start every independent task and the bottom task of every stack at the same time, each as a background agent with the prompt below.
   Start a stacked task when the task below it reports a clean result, with that task's branch tip as its baseline.
   Record each task's checkout, branch, and baseline.
4. **Land each task as it reports.**
   Start on the first report; do not wait for all of them.
   Verify the report against the checkout: the commits exist, the diff stays inside the task's scope, and the checks it reports are clean.
   Then publish under `docs/agents/git.md`: check the merged result, push, open the pull request with the Evidence bundle, wait for CI, and merge when `docs/agents/review-and-merge.md` allows it.
5. **Re-integrate when the target moved.**
   Every merge moves the target branch.
   The merged-result check merges the new tip into the task's branch.
   When that brings in commits, also run the task's tests and the type check before the push, because a clean text merge can still break behavior.
   A conflict that needs judgment goes to a fresh agent with the branch, the conflict, the intent of both sides, and the checks to run.
6. **Handle failures and keep going.**
   An agent that reports a blocker or a failed check gets one fresh agent with the failure as its handoff.
   A second failure on the same root cause stops that task: keep its checkout, record the cause, and continue the other tasks.
   Classify each failure as introduced, pre-existing, or environmental.
7. **Clean up.**
   Release the checkout of each merged task, from this session's own checkout.
   Keep the checkout of each unmerged, blocked, or review-pending task.
8. **Report.**
   One line per task: pull request URL, merge result, and any deferred coverage or recorded failure.
   Then each kept checkout and the reason.

Done when every task has merged, or is reported as blocked, waiting for approval, or waiting for review.
An open pull request is not done.

## Agent prompt

Fill every placeholder before you start the agent.

```text
You implement one task end to end, up to a local commit.
Work alone: do not start other agents.

Setup:
1. Get your own isolated checkout, as docs/agents/delegation.md describes.
   Work only there.
2. Check out <baseline commit> and create <branch>. Verify that HEAD is the baseline.
3. Install dependencies from the lockfile, and copy local settings the Project's docs name.
4. Read .rules, then the pages it routes you to for this task.

Task:
<scope, issue link, spec, allowed paths>

Stack:
<"independent", or the branch and task this one builds on>

Constraints:
- The known-red baseline ran at <baseline commit>. Failing tests there: <list>.
  Do not run the full baseline again; run the tests that cover this change.
- Reproduce first, write a fail-first test, then change the code.
- Review your own change on Standards and Spec, fix each finding, and check again.
- <test deferral, if any>
- Commit under docs/agents/git.md. Keep hooks on.
  After each commit, confirm that `git log --oneline -1` moved.
- Push, pull request, and merge belong to the coordinator. Keep your branch local.

Report:
- Checkout path, baseline, branch, and commit hashes.
- Changed files, and one paragraph on what changed and why.
- Review findings and how you resolved each one.
- Check results, each failure classed as introduced, pre-existing, or environmental.
- Any blocker, deferred coverage, or open question.
```
