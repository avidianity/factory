# no-suppressions

**Invariant**:
No source file holds an inline directive that turns off a lint or type rule, except in a list of approved exceptions that changes only at Tier 3.

**Why**:
A suppression lets an agent pass a check without fixing the cause.

**Applies when**: the Project has a linter or type checker.

**Runs in**: hook and CI.

**Failing case**:
A line with a directive that disables a lint rule for the next line.

**Passing case**:
The code is fixed, and no directive remains.

**Error text**:
`<file>:<line> suppresses <rule>. Fix the cause, or change the rule for the whole Project through an ADR.`
