# Issue tracker instructions

This page applies when you create or update issues or pull requests, or triage work.
The Project's tracker is `<tracker>`; [adapters/trackers.md](../../adapters/trackers.md) gives its commands.

## Mandatory: what belongs in the tracker

The tracker holds:

- tasks that someone will do: features, bugs, and chores;
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
| `tier-0` to `tier-4` | The Tier that the tier check gave a pull request |
| `<owner label>` | The person that the working agent works for; one label per person |

The tier check applies the `tier-*` label.
An agent never applies or removes a `tier-*` label by hand.

## Mandatory: owner labels

Before you start work on an issue, add the owner label of the person you work for.
It is the first write on that issue.
It tells others that the issue is taken, and by whom.

When an issue already has another person's owner label, do not take it.
Ask the Owner.

## Mandatory: issue bodies

- Write issue and pull request bodies in plain English for people, not in a compressed agent style.
- Use [templates/task-contract.md](../../templates/task-contract.md) for a task that an agent will do alone.
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
