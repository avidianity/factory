---
name: one-go
description: Land one task in one go. Implement, review, commit, push, open a pull request, and merge.
disable-model-invocation: true
argument-hint: The task to land
---

# one-go

Land one task through the publishing workflow in `docs/agents/git.md`.
Read that page and `docs/agents/testing.md` before you act.
Read `docs/agents/delegation.md` only when the Owner asked for delegation.
This skill runs in the current session and needs no other agent.

## Process

1. **Prepare.**
   Look at existing changes, and keep unrelated work safe.
   Use the current branch unless the Owner names another, and follow `docs/agents/git.md` for long-lived branches.
   Bring the branch up to date, then record the exact baseline commit.
2. **Implement.**
   Treat the Owner's current request as the spec.
   Reproduce the behavior first, write a fail-first test, then change the code.
   Honor an explicit test deferral as `docs/agents/testing.md` describes.
3. **Review.**
   Review your working-tree change before the first commit, on two axes:
   - Standards: does it follow the Project's documented rules?
   - Spec: does it do what the task asked, no more and no less?
   Fix each finding and review again.
4. **Respect human review.**
   When the Owner asked to review, show the change and stop until they approve.
   Otherwise, add no new approval step.
5. **Publish.**
   Follow `docs/agents/git.md` for commits, hooks, push, the pull request, the CI gate, and the merge.
   Write the pull request body from the Evidence bundle template.
   Check the merged result right before the merge.
   The merge also needs whatever `docs/agents/review-and-merge.md` requires for the Tier.
6. **Report.**
   Give the pull request URL, the merge result, and any gap.

Done when the reviewed task has merged into the target branch, or when a requested review, a required approval, or a concrete blocker is reported as pending.
A push alone is not done.
