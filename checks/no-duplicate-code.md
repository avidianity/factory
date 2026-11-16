# no-duplicate-code

**Invariant**:
A change adds no block of code that duplicates another block in the Project.
A duplicate is a block of 10 or more lines, or 100 or more tokens, that appears twice when formatting is ignored.
Test files count.
Generated files are exempt.

The thresholds are Defaults.
The Project keeps them in the check's configuration, changes them only by an ADR, and a change to them is Tier 3; see [test-oracle-guard](./test-oracle-guard.md).

**Why**:
Each copy of a block is more code to build, test, and read.
A fix that reaches one copy and misses the other is a defect.

**Applies when**: the Project has application code.

**Runs in**: hook and CI, on the lines that the change adds.

**Failing case**:
A change copies a 15-line validation block from one handler into another.

**Passing case**:
The change moves the block into one function that both handlers call.
Duplicates that existed before the change do not fail the check; their cleanup issue removes them.

**Error text**:
`<file>:<lines> duplicates <other file>:<lines>. Extract one shared unit and call it from both places.`
