# Corrections instructions

This page applies when an agent mistake repeats, when a bug escapes after merge, or when you decide how much autonomy agents get.
Its goal: each repeated mistake becomes a check at the lowest layer that can stop it.

## Mandatory: when a mistake becomes a Correction

A mistake becomes a Correction when it happens a second time, in any session.
A single mistake is fixed in its own change and needs no record.

Open one issue labelled `correction` per failure class, with [templates/correction.md](../../templates/correction.md).
Before you open one, search for an open Correction of the same class and add to it instead.

A Correction names:

- the failure: a reproducible example and the area it affects;
- the impact on users: severity and likelihood;
- the root cause: a missing invariant, a confusing API, an unclear requirement, or missing tooling;
- the lowest layer that can stop it, and why no lower layer can;
- a failing example and a passing example;
- where the check runs: hook, CI, or both.

A Correction closes only when its check or better API has merged and its failing example fails.
A rule in prose does not close a Correction, unless no lower layer can decide the case.

## Mandatory: decision order

For each Correction, take the first layer that can stop the mistake:

```text
Can we remove the confusing API or the invalid state?   -> simplify the design
Can the database enforce the rule?                      -> constraint or transaction
Can the type system rule it out?                        -> type or API shape
Can a linter or dependency rule detect it?              -> static rule
Can an automated test reproduce it?                     -> regression or property test
Can an agent drive the running product to prove it?     -> runtime verification
Is it a hard concurrency or ordering property?          -> consider model checking
Only judgment or policy is left?                        -> one short instruction, or review
```

[methods/trust-layers.md](../../methods/trust-layers.md) explains why this order holds.

## Mandatory: no rule inflation

- One rule lives in one place.
  Do not keep a rule both as a long instruction and as a check, unless each does a different job.
- When a check ships, shrink the prose rule to one line that links the check spec.
- Each check's error text names the rule and the approved alternative, so the agent can fix the mistake without reading the docs.

## Mandatory: Escaped defects

When someone finds a bug after its change merged:

1. Label the bug's issue `escaped`.
2. Link the pull request that caused it.
3. Record its severity: S1 critical, S2 major, S3 moderate, or S4 minor.
4. Record which check or review should have caught it.
5. When the same kind of bug escapes again, open a Correction.

## Mandatory: gates read the records

Autonomy grows only when the records show it is safe.
The Project's autonomy gate is open when all of these are true:

- No `escaped` bug of severity S1 or S2 from an agent-merged change in the last 30 merges.
- No `escaped` bug from a Tier 1 or 2 agent-merged change in the last 30 merges, or each one has a merged Correction.
- No open Correction for a failure class that touches a Tier 3 or 4 area.

When the gate closes, agent merges stop for every Tier.
The Owner approves each merge until the gate opens again.
The Owner can change the thresholds with an ADR.

## Mandatory: stop triggers

Stop all agent merges at once, and tell the Owner, when any of these happens:

- An agent crosses a data boundary between users or tenants, or leaks a secret.
- An agent changes a test to hide a failing behavior.
- Evidence is false or cannot be reproduced, more than once.
- Flaky CI or runtime verification prevents a trustworthy result.
- An agent-merged change causes a critical incident or an unauthorized change of data.
- Cost exceeds the limit the Owner set.
- An agent works outside its approved repository, branch, scope, or permissions.

Merges resume only after the Owner decides.

## Default: fuller measurement

The records above are light on purpose: one label per record, and evidence in pull request bodies.
For cost, cycle time, and controlled comparisons, see the Kit's `fleet/measurement.md`.
