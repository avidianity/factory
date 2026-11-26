# test-oracle-guard

**Invariant**:
A pull request that deletes a test, skips a test, removes an assertion, adds a lint or type suppression, raises a test timeout, or changes CI, policy, or check configuration is at least Tier 3.
Policy and check configuration includes the tier map, the active Autonomy gate file, the Feedback budget file, the test-scope configuration, the comment allow-list, the duplicate-code thresholds and allow-list, the Standing Decisions ADR, `.rules`, task pages under `docs/agents/`, and domain `RULES.md` files.

**Why**:
An agent can make a failing check pass by weakening the check instead of fixing the code.

**Applies when**: always.

**Runs in**: CI, from the protected base revision as an input to the risk-tier check; see [trusted-ci](./trusted-ci.md).

**Failing case**:
A pull request changes a test's expected value from 100 to 99 and marks another test as skipped, and the tier check gives Tier 1.

**Passing case**:
The tier check gives Tier 3 and names both changes.
Also passing: a pull request adds a new test or tightens an existing assertion without weakening any other check, and keeps its normal Tier.

**Error text**:
`Tier 3: <file> weakens a test or check (<what changed>). This needs approval under review-and-merge.md.`
