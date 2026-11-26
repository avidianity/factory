# no-duplicate-code

**Invariant**:
A change adds no duplicate block of application code inside the same module, except entries on the Tier-3 allow-list.
A duplicate is a block of 10 or more lines, or 100 or more tokens, that appears twice within the same module when formatting is ignored.
Test files, test fixtures, database migrations, frozen versioned schemas or public contracts, and generated files are exempt by default.
Duplication across separate domain modules is allowed by default so unrelated modules stay decoupled.

The thresholds and scope are Defaults.
The Project keeps them and its intentional-duplication allow-list in the check's configuration, changes them only by an ADR, and a change to them is Tier 3; see [test-oracle-guard](./test-oracle-guard.md).
Build this check with a token-based or syntax-tree clone detector, never a custom regular-expression script.

**Why**:
Copy-paste duplication inside one module creates drift when a fix updates one copy and misses another.
Unconditional zero-duplication across modules or tests causes premature abstractions that couple unrelated features and obscure independent tests.

**Applies when**: the Project has application code and a clone detector (Earned spec).

**Runs in**: hook and CI, on the lines that the change adds.

**Failing case**:
A change copies a 15-line validation block from one handler into another handler inside the same module.

**Passing case**:
The change moves the shared validation block into one function that both handlers in that module call.
Duplicates that existed before the change do not fail the check; their cleanup issue removes them.
Also passing: two integration tests share a 15-line fixture setup, or two separate domain modules each keep a 12-line schema mapping to stay decoupled.

**Error text**:
`<file>:<lines> duplicates <other file>:<lines> in the same module. Extract one shared unit when both belong to the same concept, or record a Tier-3 allow-list entry when sharing would couple unrelated concepts.`
