# test-oracle-guard

**Invariant**:
A pull request that deletes a test, skips a test, removes an assertion, adds a lint or type suppression, raises a test timeout, or changes CI or check configuration is at least Tier 3.

**Why**:
An agent can make a failing check pass by weakening the check instead of fixing the code.

**Applies when**: always.

**Runs in**: CI, as an input to the risk-tier check.

**Failing case**:
A pull request changes a test's expected value from 100 to 99 and marks another test as skipped, and the tier check gives Tier 1.

**Passing case**:
The tier check gives Tier 3 and names both changes.

**Error text**:
`Tier 3: <file> weakens a test or check (<what changed>). This needs approval under review-and-merge.md.`
