# behavior-has-test

**Invariant**:
A pull request that changes application source files also changes or adds a test file, or its body says "No behavior change" with a reason.

**Why**:
A behavior change without a test can break again without anyone seeing it.

**Applies when**: the Project has application code.

**Runs in**: CI, on pull request events.

**Failing case**:
A pull request changes a source file and no test file, and its body has no "No behavior change" line.

**Passing case**:
The same change also adds a test, or it is a rename with "No behavior change: rename only."

**Error text**:
`This pull request changes source files but no tests. Add a fail-first test, or state "No behavior change" with a reason.`
