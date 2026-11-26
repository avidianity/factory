# Decision log

The decision log is one file where the Coordinator records each Decision the Owner made.
Every session reads the same answer, so the Owner answers each question once.

## Where it lives

Keep it outside any Project, in the Coordinator's own folder, because Decisions often cross Projects.
A Decision that is part of one Project's architecture goes into that Project as an ADR.
Pre-approved autonomous boundaries for one Project live in that Project's Standing Decisions ADR; see [core/agents/review-and-merge.md](../core/agents/review-and-merge.md#standing-decisions).

## Entry format

```text
## <short title>

Date: <date and time, with time zone>
Asked: <the question, as the Owner saw it>
Answer: <the Owner's answer, quoted>
Applies to: <Projects, areas, or task kinds>
Until: <an end date or condition, or "until changed">
```

## Rules

- Record the Owner's words, not a summary that changes their meaning.
- Before you ask the Owner anything, search the log.
  When an entry answers it, use the entry, and name it in your report.
- When the Owner changes a Decision, add a new entry and mark the old one "replaced by <title>".
  Do not delete history.
- Keep a short section at the top: "What the Coordinator decides alone".
  Everything not listed there goes to the Owner.
- A "yes" from the Owner to a recommendation approves the recommendation as written, nothing more.

## What the Coordinator may decide alone

Start with an empty list, and let the Owner add to it.
Tier 3 and Tier 4 concerns never belong on this list.
Typical entries:

- Which Worker gets which task.
- When to release or restart a Worker.
- Retry of a failed CI run that failed for an environmental reason.
- Fixes for security problems that the Owner pre-approved as a class.
