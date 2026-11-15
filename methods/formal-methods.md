# State machines and formal methods

Formal methods are strong and costly.
This page says when they are worth it and how to keep them honest.

## Map the implicit state machine first

A state machine often hides in status fields, timestamp flags, booleans, enums, webhook handlers, and job logs.
Map the real one from the code before you model anything.

Prompt for a repository audit:

```text
Find implicit state machines in this repository.
Look for status fields, combinations of booleans, timeout and retry flags,
workflow updates, webhooks, events, job handlers, routes, and database updates.
For each one: derive states and transitions from the code, citing files and lines;
find impossible, ambiguous, and unhandled states; list side effects;
show guards and their authorization or tenant dependencies; draw it;
and propose the smallest executable negative test before any library or framework.
Do not infer intended behavior from names alone. Mark unknown product decisions.
```

## Candidates that often pay off

- Money or credits: reserve, settle, refund, retry, cancel.
  Invariant: no double charge and no unauthorized negative balance.
- Multi-step forms: draft, submitted, rejected, approved.
  Invariant: no invalid transition and no duplicate submission.
- Background jobs: queued, running, retried, succeeded, failed, cancelled.
  Invariant: one business request causes at most one external side effect.
- Exports: requested, authorized, generated, delivered, expired.
  Invariant: no disclosure across tenants, including on retry and with stale links.
- Long-running data jobs: upload, convert, query, ready, purge.
  Invariant: a stale dataset is never used in place of the current authorized one.

These are hypotheses.
Start from the real states and the bugs you have seen.

## The order of work

1. Extract the real state variables and transitions, including external events and retries.
2. Settle the intended behavior with the Owner.
3. List invariants:
   - safety: something bad never happens;
   - liveness: something good eventually happens, under stated assumptions;
   - consistency: related records agree.
4. Find the small slice of concurrency or ordering that matters.
5. Write table-driven transition tests and fault-injection reproductions.
6. Add a model checker only when exhaustive exploration of a bounded model shows more than hand-picked sequences.
7. Tie the model to the code with property tests, integration tests, or trace mappings.
8. Version the model with the code, and fail on drift.

Example invariants:

```text
Safety: a submitted form never returns to an editable draft without a recorded revision.
Safety: a user without the approver role never approves a record.
Safety: one logical paid request never creates two successful charges.
Liveness: a reserved charge is eventually settled, refunded, or escalated.
Consistency: a report result matches its declared dataset and schema versions.
Consistency: a job marked succeeded means its business effect is committed.
```

Each statement needs its assumptions.
A property that ignores an administrator override or manual reconciliation can be too strong, or wrong.

## Admission rule

Use a formal model only when all of these are true:

- A small set of states and transitions captures a costly risk.
- An invariant can be stated without ambiguity.
- Normal tests cannot cover the relevant interleavings at a sensible cost.
- Someone will review the model's assumptions.
- A test or a trace connects the model's result to the real code.
- The model can be kept current when the code changes.

## Keeping a formal check honest

1. Choose a real property whose violation harms users and that ordinary tests reach poorly.
2. Choose the right mechanism: model checking for orderings and interleavings, code-level verification for input ranges, proof for narrow claims, or generated property tests when nothing stronger fits.
3. Show that a broken variant fails the check.
4. When the check fails, never weaken the property to get a pass.
5. Record the model's assumptions, its bounds, the code it covers, and the last commit it matched.
6. Give a reproducible command and a scope statement: what was proved, how, and what was not covered.
7. Update the model when the code changes; a stale proof is worse than none.

A finite model can expose a missed transition or an unexpected interleaving.
It does not prove that the code matches the model.
