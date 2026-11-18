# Issue tracker instructions

This page applies when you create or update issues or pull requests, or triage work.
The Project's tracker is `<tracker>`; [adapters/trackers.md](../../adapters/trackers.md) gives its commands.
The pinned planning skills read this page to find the tracker, its labels, and its operations.

## Mandatory: what belongs in the tracker

The tracker holds:

- tasks that someone will do: features, bugs, and chores;
- Specs, Tickets, and Maps, as [planning.md](./planning.md) describes;
- Corrections, labelled `correction`;
- Escaped defects, labelled `escaped`;
- pre-existing failures, labelled `needs-triage`, one issue per root cause.

The tracker does not hold:

- decisions; those go in ADRs;
- domain terms; those go in glossaries;
- notes that only matter to one session.

## Mandatory: labels

Bootstrap creates these labels:

| Label | Meaning |
| ----- | ------- |
| `correction` | A repeated agent mistake that needs a check or a better API |
| `escaped` | A bug found after its change merged |
| `needs-triage` | A failure or report that nobody has classified yet |
| `needs-info` | A report that waits for more information from its author |
| `ready-for-agent` | A fully specified issue that an agent can do alone |
| `ready-for-human` | An issue that needs a person to do it |
| `wontfix` | An issue that nobody will act on |
| `bug` | A category for `triage`: something is broken |
| `enhancement` | A category for `triage`: a new feature or an improvement |
| `wayfinder:map` | A Map |
| `wayfinder:research`, `wayfinder:prototype`, `wayfinder:grilling`, `wayfinder:task` | The type of a decision ticket on a Map |
| `budget` | A Feedback loop took 80% of its Feedback budget or more |
| `cleanup` | Old comments, dead code, or duplicate code in one module that existed before Bootstrap |
| `tier-0` to `tier-4` | The Tier that the tier check gave a pull request |
| `<owner label>` | The person that the working agent works for; one label per person |

The tier check applies the `tier-*` label.
An agent never applies or removes a `tier-*` label by hand.

Triage roles use labels of the same name: `needs-triage`, `needs-info`, `ready-for-agent`, `ready-for-human`, and `wontfix`.
Pull requests are not a triage surface: `triage` reads issues only.

## Mandatory: operations

<The operations table for the Project's tracker, copied from adapters/trackers.md, with the Project's repository filled in.>

When a skill says "publish to the issue tracker", create an issue.
When a skill says "fetch the ticket", read the issue with its comments.

## Mandatory: Specs, Tickets, and Maps

- A Spec is one issue.
  `to-spec` writes it and applies `ready-for-agent`.
- Each Ticket is one issue and a sub-issue of its Spec.
  Its body states what it delivers, its acceptance criteria, and its Expected Tier.
- A Blocking edge uses the tracker's native "blocked by" relationship.
  Write a `Blocked by` line in the body only when the tracker has no such relationship.
- A Ticket is on the Frontier when every Ticket that blocks it is closed, and it has no owner label.
  An agent claims a Ticket with the owner label, as the next section says.
- A Map is one issue labelled `wayfinder:map`.
  Its decision tickets are its sub-issues, each with one `wayfinder:` type label and no triage label.
  A session claims a decision ticket by assigning it to the person it works for, before any other work.
- Never close or edit a parent issue while you split it.

## Mandatory: Wayfinding operations

`wayfinder` reads this section.
A Map is one issue, and its decision tickets are its sub-issues.

- **Map**: create the issue with the label `wayfinder:map`.
- **Decision ticket**: create it as a sub-issue of the Map, with one `wayfinder:` type label.
- **Blocking**: add each edge with the "Add a Blocking edge" operation.
- **Frontier query**: list the Map's sub-issues, keep the open ones, and drop each one with an open blocker or an assignee.
  The first one in Map order wins.
- **Claim**: assign the decision ticket to yourself, as the first write.
- **Resolve**: comment the answer, close the issue, then add one line with the gist and a link to the Map's "Decisions so far".

## Mandatory: owner labels

Before you start work on an issue, add the owner label of the person you work for.
It is the first write on that issue.
It tells others that the issue is taken, and by whom.

When an issue already has another person's owner label, do not take it.
Ask the Owner.

## Mandatory: issue bodies

- Write issue and pull request bodies in plain English for people, not in a compressed agent style.
- Use [templates/task-contract.md](../../templates/task-contract.md) for a task that an agent will do alone and that no Spec covers.
- Use [templates/correction.md](../../templates/correction.md) for a Correction.
- Link related issues, and state which issue blocks which.
- Never put a secret, a token, or personal data in an issue or pull request.

## Default: stale specs

A spec that nobody touched for about two weeks may be stale.
Before you implement it, compare it with the current code and docs, and ask the Owner about each difference.

## Mandatory: closing

- A pull request with `Closes #<issue>` closes the issue when it merges.
- Close a parent issue when every child is closed and no caveat remains.
- A Correction closes only when its check or better API has merged.
