# Trust layers

Agents follow the environment more reliably than they follow instructions.
This page explains the layers of trust, the principles that follow from them, and how to judge a new check.

## The layers

From most trusted to least trusted:

1. **API and architecture.**
   A design in which the wrong thing cannot be expressed.
   Example: a function that takes a verified tenant value, so no caller can pass a raw identifier.
2. **Static checks.**
   Types, lint rules, dependency rules, and schema constraints that reject the wrong thing before it runs.
3. **Tests and runtime verification.**
   Automated proof that the running code does the right thing for chosen inputs.
4. **Model checking and proofs.**
   Exhaustive exploration of a small model, or a proof of a narrow claim.
   Strong, but only about the model, and costly to keep current.
5. **Instructions and review.**
   Prose rules and human or agent judgment.
   Necessary for intent and policy, and the easiest layer for an agent to miss.

Put each rule at the highest layer in this list that can decide it.
[core/agents/corrections.md](../core/agents/corrections.md#mandatory-decision-order) turns this into an order of questions.

## Principles

1. **Enforce invariants at the lowest sensible layer.**
   Database constraints, transactions, types, and runtime guards handle the problems they can prove.
2. **Prefer conventions and simple boundaries.**
   Do not add layers of interfaces only to look clean.
   A direct query can be the clearest code.
3. **Test behavior, not implementation.**
   A good test fails when a rule that a user can see breaks.
4. **Make checks explain the fix.**
   An error names the rule and the approved alternative.
5. **Green CI never replaces product intent.**
   Every task has explicit acceptance criteria.
6. **Separate who implements from who approves.**
   Risky changes need an approval that the implementer cannot give.
7. **Keep the system observable, budgeted, and stoppable.**
   No uncontrolled swarms of agents.
8. **Prove one loop before you automate the outer loop.**
   A small verified throughput beats a large unverified output.

## What does not count as proof

- The number of pull requests.
- Green CI on a revision other than the one that merges.
- An agent's own report or a confident explanation.
- Agreement between several agents; they can share one blind spot.
- A screenshot without a check of the stored effect.
- A low revert rate; bugs are often fixed forward, not reverted.

## Judging a new check

Score each proposed check from 0 (none) to 3 (strong) on:

1. A specific, costly, or repeated problem exists, with evidence.
2. The check is enforceable and rejects a planted failing case.
3. False positives and exceptions are rare and cheap to maintain.
4. The benefit is measurable, in fewer escaped defects or less review time.
5. The added cost is acceptable for the risk.
6. The owner, the removal rule, and the permission boundary are clear.

Never approve on total score alone.
A security or data-safety problem is a veto on its own.
Require at least 2 on criteria 1 and 2 before you build the check.
Prefer cheap checks that remove real, repeated work.

## Scaling agents

More agents raise output only up to the slowest shared stage: specification, CI capacity, test environments, review, or merge coordination.
When that stage saturates, more agents add queue time and conflicts, not accepted work.
Measure accepted outcomes, not generated pull requests or tokens.
