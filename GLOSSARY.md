# Factory glossary

This glossary defines the kit's language.
Every kit page and every bootstrapped project uses these terms as written.
A project keeps its own domain glossary beside this language; see [core/agents/domain.md](./core/agents/domain.md).

## Kit and project

**Kit**:
This repository: the documents that set up a Project for agent development.
_Avoid_: template, boilerplate, starter.

**Project**:
A repository that an agent bootstrapped from the Kit.
_Avoid_: app, target repo.

**Bootstrap**:
The one-time process in [BOOTSTRAP.md](./BOOTSTRAP.md) that fits the Kit to a Project.
_Avoid_: install, init, scaffold.

**Survey**:
The read-only first step of Bootstrap and Upgrade, which finds the Project's stack, existing rules, checks, docs, tracker, and recurring failures.

**Adoption plan**:
The plain-language list that Bootstrap writes for an existing Project: Kit rules that are missing, existing rules that conflict, and check specs that already exist in some form.
The Owner decides each conflict in it.

**Upgrade**:
The process in [UPGRADE.md](./UPGRADE.md) that applies changes from a newer Kit version to a Project.
_Avoid_: sync, update.

**Kit version**:
A Kit release tag such as `v1.2.0`, recorded in the Project's rules file.

**Stack-convention conflict**:
A case where the Project's language, framework, or tool expects something that a Kit rule forbids or contradicts.
The agent asks the Owner and records the answer as an ADR.

## People and decisions

**Owner**:
The human who is responsible for a Project.
The Owner makes every Decision and gives every approval that a tier requires.
_Avoid_: user, operator, client.

**Decision**:
A choice about product behavior, data, cost, security, or architecture.
The Owner always makes it, in every Review mode.
The agent gives a recommendation and phrases the choice so that the Owner can judge it.

**Standing Decision**:
An Owner-approved boundary for routine choices, recorded in one ADR linked from `.rules`, with explicit scope, limits, and expiry.
Tier 3 and Tier 4 concerns never become Standing Decisions.

**Code review**:
Reading a diff to judge it.
A human or an Independent reviewer does it, as the Review mode says.
_Avoid_: using "review" alone when the meaning is a Decision.

**Review mode**:
The Project's setting for who does Code review in each tier: **Always**, **Critical only**, or **Never**.
[core/agents/review-and-merge.md](./core/agents/review-and-merge.md) defines each one.

**Independent reviewer**:
An agent session that did not see the implementation, ideally on a model from a different vendor, which checks a diff against its spec and its check specs.
_Avoid_: second agent, reviewer agent.

**Owner guide**:
The Project page `docs/owner-guide.md` that tells the Owner which skill to start in each situation, what to type, what comes back, and what to approve.
Agents read `.rules`; the Owner reads the Owner guide.
_Avoid_: tutorial, manual, user guide.

**Risk summary**:
A plain-language page for the Owner: what changes for people, what can go wrong, how to undo it, and the evidence.
It contains no code.

## Change and risk

**Tier**:
The risk class of a change, from 0 (mechanical) to 4 (critical).
A check computes it deterministically from changed paths, reverse dependencies, and diff signals against the protected base revision; the agent never sets or lowers it.
_Avoid_: severity, priority.

**Check spec**:
A document in [checks/](./checks/) that states one invariant, failing cases, passing cases, where the check runs, and the error text.
A Project builds every Day-1 core check spec at Bootstrap, and builds Earned check specs when a suitable tool exists and the Owner opts in, or on a Correction.

**Check**:
The executable form of a check spec in a Project: a hook, a lint rule, a test, or a CI job.

**Evidence bundle**:
The proof that a change works, kept in the pull request body: the tier, the fail-first test, check results, the Safety proof, and runtime evidence.

**Fail-first test**:
A test that fails on the code before the change and passes after it.

**Correction**:
An issue for an agent mistake that happened more than once.
It names the failure, its root cause, and the lowest layer that can stop it.
It closes only when a check or a better API ships.
_Avoid_: lesson, retro item.

**Escaped defect**:
A bug that someone found after its change merged.
Its issue carries the `escaped` label and links the pull request that caused it.

**Known-red baseline**:
The list of failing tests that a session records before its first edit.

**Feedback loop**:
One automatic run of checks that an agent or a human waits on before work continues.
A Project has three: the Git hook, the Relevant tier of the check ladder, and the CI gate on each pull request.
The full suite is not a Feedback loop, because it runs only when the Owner asks.

**Feedback budget**:
The maximum wall-clock time of one Feedback loop.
_Avoid_: speed limit, SLA, performance budget.

## Planning

**Spec**:
One tracker issue that describes a whole change from the user's side: the problem, the solution, user stories, implementation and testing decisions, and what is out of scope.
_Avoid_: PRD, design doc.

**Ticket**:
One thin slice of a Spec, published as its own issue, that cuts through every layer and fits one fresh agent session.
_Avoid_: subtask, story.

**Blocking edge**:
A link that says one Ticket cannot start until another Ticket is closed.

**Frontier**:
The open Tickets whose blockers are all closed and that nobody has claimed with an owner label or an assignee.

**Map**:
One tracker issue that charts work too big for one session, with decision tickets as its sub-issues.
Each decision ticket settles a question, not a slice of code.

**Expected Tier**:
The Tier that a Ticket states from the tier map before code exists.
The tier check on the final revision replaces it.

**Design sketch**:
The types, signatures, and module boundaries of a change, written as code with no bodies, before the implementation.
_Avoid_: design doc, architecture doc.

**Integration branch**:
The one short-lived branch on which every Ticket that `implement-spec` builds merges, and which lands as one pull request.

**Safety fact**:
The one fact that a change is safe because of, proven by a run of the real code.

## Verification

**Verification contract**:
The Project's repeatable way to launch the application, check that it is healthy, drive one user journey, assert the result, keep evidence, and clean up.

**Feature map**:
One page per user-visible feature that names its entry point, preconditions, actions, success invariants, evidence, and cleanup.

## Fleet

**Fleet**:
Many agent sessions that work at the same time under one coordinator.
[fleet/](./fleet/) describes it.
_Avoid_: swarm.

**Coordinator**:
The session that plans, briefs, watches, and relays, and does not build.
In a large Fleet, area coordinators report to one main coordinator.
_Avoid_: orchestrator, overseer, manager.

**Worker**:
A session that does one task in its own isolated checkout and reports only to its coordinator.

**Brief**:
The self-contained message that starts a Worker on a task.

**Decision log**:
The one file where the Fleet records every Owner Decision, so that every session reads the same answer.
