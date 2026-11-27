# risk-tier

**Invariant**:
Every pull request gets a Tier from 0 to 4, computed deterministically from three inputs against the protected base revision (see [trusted-ci](./trusted-ci.md)):

1. **Changed paths**: each changed path matched against the Project's tier map on the base revision.
   A path in no list is Tier 2.
2. **Reverse dependencies**: any changed file that a Tier 3 or Tier 4 path transitively imports or depends on inherits that Tier, computed from the Project's dependency graph.
   When a stack lacks a static dependency graph, mark shared foundation folders Tier 3 in the tier map.
3. **Diff signals**: dependency manifest or lockfile changes are at least Tier 2; schema migrations, new external network endpoints, secret reads, and authentication or authorization guards outside known risk folders are at least Tier 3; bulk data change scripts and irreversible data-dropping operations (such as dropping populated tables or columns or truncating tables) are Tier 4.
   [test-oracle-guard](./test-oracle-guard.md) adds Tier 3 signals for weakened tests, checks, or policy files.

The highest Tier across all three inputs applies.
The check applies the `tier-<n>` label and replaces any `tier-*` label that someone else set.
When the Project uses Autonomy gates, this check (or a companion required check reading the protected base revision) compares the computed Tier against the Project's active gate policy file and blocks merge without the required approvals whenever the Tier exceeds the active gate.
Never gate the Tier on an agent or a language model; semantic impact analysis belongs in `blast-radius` from Tier 2 onward.

**Why**:
An agent that sets its own Tier can lower it to merge without approval.
Path lists alone also miss shared utilities imported by sensitive modules and risky changes added outside known folders.

**Applies when**: always.

**Runs in**: CI, on every pull request event, executing its code and reading its tier map from the protected base revision.

**Tier map**:
The Project keeps the tier map in one file, such as `docs/checks/tier-map.md` or a config file that the check reads from the base revision.
Changes to the tier map file, the tier check, and CI configuration are Tier 3.

**Failing case**:
A pull request changes a file under the authentication folder, which the tier map lists as Tier 3, and someone set the label `tier-1`.
Also failing: a pull request edits a shared date helper that the payments module (Tier 3) imports, and the check labels it `tier-1` or `tier-2` from its path alone.

**Passing case**:
The check replaces `tier-1` with `tier-3`, naming the matched path, reverse dependency, or diff signal, and the merge contract requires approval.
Also passing: a pull request edits an isolated display helper that no Tier 3 or Tier 4 path imports and triggers no diff signal, and the check keeps its path-based Tier.

**Error text**:
`Tier <n>: <path> matched <tier map rule | reverse dependency from <critical path> | diff signal <signal>>. This Tier needs <approval rule from review-and-merge.md>.`
