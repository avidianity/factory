# migration-apply

**Invariant**:
Every change to SQL migration, schema, or migration-tool files applies cleanly on a disposable instance of the Project's actual target database engine in two modes: clean creation from an empty database, and upgrade from each supported existing schema baseline or snapshot.
The upgrade run preserves synthetic fixture data and enforces declared constraints without error.
When the Project tracks a canonical schema file or migration checksum state, applying migrations leaves no unreported schema drift.

**Why**:
Migrations that look valid in review fail on real engines due to dialect errors, transaction-block restrictions, missing prerequisites, existing rows violating new constraints, or drift between fresh installs and upgraded databases.

**Applies when**: the Project has SQL migrations and no batteries-included framework or library supersedes this migration workflow in a stack ADR.

**Runs in**: CI when migration, schema, or database-tool files change, and locally at handoff for those changes; never inside unrelated application Relevant Feedback loops.

**Failing case**:
A pull request adds a `NOT NULL` column with no default or backfill to an existing table that has rows in a supported baseline snapshot, causing the upgrade run to fail.
Also failing: a migration upgrades an existing database cleanly but fails on an empty database because it depends on an untracked manual object or stale schema snapshot.

**Passing case**:
Migrations apply cleanly from an empty disposable database and from every supported baseline snapshot on the target engine, preserve synthetic fixture data, keep constraints intact, and report zero schema drift.
Also passing (false-positive trap): a migration uses an engine-required non-transactional directive that the migration tool executes outside a transaction block, or re-runs an explicitly declared repeatable script idempotently.
Also passing (false-positive trap): a documentation-only or non-database change skips migration verification.

**Error text**:
`Migration verification failed in <clean | baseline <name>> mode on <file>: <engine or tool error>. Fix the migration so both empty creation and supported upgrades succeed cleanly; see sql.md.`
