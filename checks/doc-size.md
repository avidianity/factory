# doc-size

**Invariant**:
`.rules` and every page in its "Always read" column are each under 32,000 bytes.

**Why**:
Some agents cut project instructions off near that size.
A rule that an agent never sees is a rule it cannot follow.

**Applies when**: always.

**Runs in**: hook and CI.

**Failing case**:
`.rules` is 33,000 bytes.

**Passing case**:
`.rules` is 20,000 bytes and each always-read page is under 32,000 bytes.

**Error text**:
`<file> is <n> bytes; the budget is 32000. Split it or move detail to a reference page. Never raise the budget.`
