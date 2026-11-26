# trusted-ci

**Invariant**:
Policy-enforcing CI checks execute their code and read their policy files from the protected base revision, never from the pull request head.
Policy-enforcing checks include [risk-tier](./risk-tier.md), [test-oracle-guard](./test-oracle-guard.md), and [evidence-bundle](./evidence-bundle.md).
Policy files include the tier map, Feedback budgets, allow-lists, and check configuration.

Those checks read the pull request's changed file list and diff strictly as untrusted data.
They never execute code or install dependencies from the pull request head in a privileged base-branch context.

Forge-enforced ownership and branch protection rules require approval from a non-agent Owner identity for any pull request that changes CI workflows, check scripts, policy files, or the Project's rules files.
Agent credentials cannot approve pull requests or bypass branch protection.

**Why**:
When CI runs check scripts from the pull request head, an agent can edit a workflow, a check script, or the tier map to label a risky change as Tier 1 and merge without approval.

**Applies when**: always.

**Runs in**: CI, on every pull request event, backed by forge branch protection and code-ownership rules.

**Protected files**:
The Project keeps one list of protected policy and CI paths in its forge ownership rules: CI workflows, check scripts, the tier map, the Feedback budget file, check allow-lists, the Standing Decisions ADR, and `.rules` with its task pages.

**Failing case**:
A pull request modifies a CI workflow or the tier map to downgrade an authentication change to Tier 1, and CI executes the modified script from the pull request head or lets the pull request merge without non-agent Owner approval.

**Passing case**:
The policy check runs from the protected base revision, reads the base tier map, labels the pull request Tier 3, and forge-enforced ownership blocks the merge until a non-agent Owner approves.
Also passing: a pull request adds application code and tests without touching CI or policy paths, and passes the control-plane check from the base revision without requiring ownership approval.

**Error text**:
`Tier 3: <path> modifies protected CI or policy configuration. Control-plane checks ran from <base revision>; non-agent Owner approval is required to merge.`
