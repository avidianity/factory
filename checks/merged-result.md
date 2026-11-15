# merged-result

**Invariant**:
The whole-repository checks run on the tree that a merge will produce, right before each merge.

**Why**:
Two pull requests can each pass alone and break the base together, for example by adding two records with the same number.

**Applies when**: always.

**Runs in**: the merge step of the landing workflow, or the forge's merge queue when the Project uses one.

**Failing case**:
Two pull requests each add ADR 0042.
The second one merges without a check on the merged tree.

**Passing case**:
Before the second merge, the agent merges the base into the branch, the record-numbers check fails, and the agent renumbers the ADR.

**Error text**:
The error text of the whole-repository check that failed.
