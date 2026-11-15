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

**Risk summary**:
A plain-language page for the Owner: what changes for people, what can go wrong, how to undo it, and the evidence.
It contains no code.

## Change and risk

**Tier**:
The risk class of a change, from 0 (mechanical) to 4 (critical).
A check computes it from the changed paths; the agent never sets or lowers it.
_Avoid_: severity, priority.

**Check spec**:
A document in [checks/](./checks/) that states one invariant, one input that must fail, one input that must pass, where the check runs, and the error text.
A Project builds each check spec as a real check in its own stack.

**Check**:
The executable form of a check spec in a Project: a hook, a lint rule, a test, or a CI job.

**Evidence bundle**:
The proof that a change works, kept in the pull request body: the tier, the fail-first test, check results, and runtime evidence.

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
