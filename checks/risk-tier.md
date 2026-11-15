# risk-tier

**Invariant**:
Every pull request gets a Tier from 0 to 4, computed from its changed paths with the Project's tier map.
The highest Tier among the changed paths applies.
A path in no list is Tier 2.
The check applies the `tier-<n>` label and replaces any `tier-*` label that someone else set.

**Why**:
An agent that sets its own Tier can lower it to merge without approval.

**Applies when**: always.

**Runs in**: CI, on every pull request event.

**Tier map**:
The Project keeps the tier map in one file, such as `docs/checks/tier-map.md` or a config file that the check reads.
Changes to the tier map file, the tier check, and CI configuration are Tier 3.

**Failing case**:
A pull request changes a file under the authentication folder, which the tier map lists as Tier 3, and someone set the label `tier-1`.

**Passing case**:
The check replaces `tier-1` with `tier-3`, and the merge contract then requires approval.

**Error text**:
`Tier <n>: <path> matches <tier map rule>. This Tier needs <approval rule from review-and-merge.md>.`
