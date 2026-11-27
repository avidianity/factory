# migration-history

**Invariant**:
Every applied versioned migration file on the protected base branch is immutable: a pull request never edits, renames, or deletes an applied versioned migration without an explicit Tier 3 or Tier 4 Owner-approved override (such as an ADR-recorded baseline consolidation).
Every versioned migration has a unique identifier and deterministic ordering under the Project's migration scheme (sequential or timestamped), verified on the merged tree before merge; see [merged-result](./merged-result.md).
Where the Project's migration tool records checksums or history manifests, committed migration history passes its integrity check.
Explicitly declared repeatable script paths (such as idempotent views or routines) are exempt from immutability.

**Why**:
Editing an applied migration causes silent schema divergence between environments that already applied the old file and fresh environments that apply the edited file.
Duplicate or misordered migration identifiers from parallel branches break migration runners and produce non-deterministic schemas.

**Applies when**: the Project has SQL migrations and no batteries-included framework or library supersedes this migration workflow in a stack ADR.

**Runs in**: CI, on pull request events against the protected base revision, and in the merged-result check before each merge.

**Failing case**:
A pull request edits a column type inside a versioned migration file that already exists on the protected base branch.
Also failing: two branches add migrations with the same identifier (`0042_add_orders.sql` and `0042_add_invoices.sql`), or a branch adds a migration whose timestamp sorts before an already-merged migration when the Project requires monotonic ordering, and tries to merge without renumbering.

**Passing case**:
A pull request leaves applied migrations untouched and adds a new corrective versioned migration with the next unique identifier.
Also passing (false-positive trap): a pull request edits an unapplied versioned migration file that was introduced in the same pull request and does not exist on the protected base branch.
Also passing (false-positive trap): a pull request updates an idempotent view inside the Project's explicitly declared repeatable scripts path.

**Error text**:
`Applied migration <file> was modified, renamed, or deleted. Keep applied versioned migrations immutable and add a new corrective migration; see sql.md.`
`Migration identifier <id> conflicts with or violates deterministic ordering against <other file>. Assign a unique next identifier on the merged tree; see sql.md.`
