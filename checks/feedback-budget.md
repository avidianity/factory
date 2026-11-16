# feedback-budget

**Invariant**:
Each Feedback loop finishes within its Feedback budget, measured in CI on the Project's standard runner.
The Default budgets are 30 seconds for the Git hook, 2 minutes for the Relevant tier, and 10 minutes for the CI gate.
When a loop takes 80% of its budget or more, the check opens one issue with the `budget` label for that loop, or updates the open one.
When a loop takes more than its budget, the check fails.

**Why**:
Code grows with every change, and each Feedback loop grows with it.
A loop that gets a little slower each week stops nobody until agents and humans wait minutes for each answer, or skip the loop.

**Applies when**: always.
Skip a loop that the Project does not have.

**Runs in**: CI, on every pull request.

**How CI measures each loop**:

- Git hook: a CI job runs the hook command on the files that the pull request changes, and records its wall-clock time.
- Relevant tier: a CI job runs the affected-tests command on the files that the pull request changes, and records its wall-clock time.
- CI gate: a final required job records the time from the start of the first required job to the end of the last one.

Local times are not evidence, because local machines differ in speed.

**Budget file**:
The Project keeps its budget numbers in one file that the check reads.
The numbers change only by an ADR.
A change to the budget file is Tier 3; see [test-oracle-guard](./test-oracle-guard.md).

**Failing case**:
The CI gate takes 11 minutes on a pull request.

**Passing case**:
The CI gate takes 7 minutes, and the check opens no issue.
Also passing: the CI gate takes 8 minutes 30 seconds, and the check opens a `budget` issue for the CI gate.

**Error text**:
`<loop> took <time>; its Feedback budget is <budget>. Make the loop faster in this pull request. Never raise the budget to pass; see testing.md.`
