# SQL engineering instructions

This page applies when you write or review raw SQL queries, SQL schemas, migrations, data backfills, or custom SQL data-access code.
It does not apply where a batteries-included framework or library owns the database layer with native conventions that contradict these rules; follow that framework's conventions per [engineering.md](./engineering.md#mandatory-stack-convention-conflicts), and apply this page only to explicit raw-SQL escape hatches or compatible parts when the Owner adopts it.
Engine versions, drivers, migration tools, local database commands, and naming, key, precision, and temporal conventions live in `<database docs path>` and ADRs.
Verify every engine-specific or driver-specific behavior against the installed version before you write code; see [engineering.md](./engineering.md#mandatory-external-facts).

## Mandatory: schema ownership and migrations

- Keep one authoritative schema-change workflow per database.
  Never mix untracked manual schema edits with versioned migrations.
- Version schema changes and required reference-data changes alongside application code.
- Give every versioned migration a unique identifier, deterministic ordering, recorded execution, and detection of modified applied history; see [migration-history](../../checks/migration-history.md).
- Keep applied versioned migrations immutable.
  Fix mistakes in applied migrations with new corrective migrations, never by editing applied history.
- Keep versioned migrations separate from explicitly supported repeatable scripts (such as idempotent views or routines) that the migration tool manages under repeatable rules.
- When adopting migrations on an existing database, establish and record an explicit baseline before adding new migrations.
- Detect and resolve schema drift between declared migrations and target environments before applying further changes.
- Serialize migration execution across processes and deployment nodes with a lock or single-runner gate.
- Handle partial migration failures explicitly.
  Inspect the recorded and actual database state and repair forward instead of re-running blindly.
- Resolve parallel-agent migration identifier or ordering conflicts on the merged tree before merge; see [git.md](./git.md#mandatory-check-the-merged-result-before-a-merge).
- Avoid blanket `IF NOT EXISTS` or `IF EXISTS` clauses in versioned migrations when they conceal unexpected schema state or drift.

## Mandatory: breaking changes and deployment

- Schema migrations are at least Tier 3; bulk data changes and irreversible operations are Tier 4; see [review-and-merge.md](./review-and-merge.md#mandatory-tiers).
- Assess compatibility against every deployed application version, background worker, and scheduled job that reads or writes the database.
- Assess lock modes, table rewrites, expected duration, and transaction support against the actual database engine and version.
- Remove old tables, columns, constraints, or compatibility paths only after every active consumer no longer depends on them.
- For changes that require continuous availability, roll out in four phases: expand, migrate or backfill, switch, and contract.
- Define deployment order, concurrent-write handling, verification gates, and the application rollback window before rollout.
- When downtime is acceptable, use an Owner-approved maintenance window instead of imposing multi-phase rollout machinery on every change.

## Mandatory: recovery

- Distinguish five recovery mechanisms: transaction rollback, application rollback, reverse migration, forward correction, and backup restoration.
- Match the recovery plan to the risk of the change and record it in the pull request Evidence bundle or Risk summary.
- Mark irreversible changes explicitly in the migration, pull request, and Risk summary.
  Recreating a dropped table or column does not recover lost data.
- Before any destructive schema or data change, require tested restore procedures and explicit recovery limits (data-loss window, restore time, and point-in-time scope).
  An untested backup is not a recovery plan.
- Never assume a transaction makes a schema change reversible across engines or after commit.

## Default: recovery strategy

- Prefer forward correction in production when reverting schema or data is riskier than applying a targeted fix.
- Write reverse migrations only when the database engine supports reversal and reversing the change is meaningful for live data.

## Mandatory: data changes and concurrency

- Run large dataset backfills in bounded, resumable batches with progress tracking, safe retries, throttling, and verified postconditions.
- Execute every statement inside a transaction on the transaction-scoped connection or handle, never on a shared pool connection.
- Choose transaction isolation, locking, atomic updates, conflict handling, and uniqueness constraints to match the business invariant.
- Handle retryable database errors (such as deadlocks or serialization conflicts) with bounded retries around the full transaction unit.
  Ensure retries never trigger duplicate external side effects.
- Before running a bulk update or bulk delete, verify the intended row scope and expected affected-row count.
  A `WHERE` clause alone is not proof of safety.
- Never run a bulk data change or an irreversible production operation alone; follow [security.md](./security.md#mandatory-forbidden-actions) and [review-and-merge.md](./review-and-merge.md#mandatory-review-modes).

## Mandatory: query safety

- Parameterize every data value.
  Never build SQL values through string concatenation or raw string interpolation.
- Handle dynamic identifiers and dynamic SQL structure (such as sort columns, sort order, or optional clauses) only through explicit allow-lists and the driver's safe identifier or fragment facility.
  See [sql-parameterization](../../checks/sql-parameterization.md) when static parameterization checks are enabled.
- Define each query's expected row cardinality (zero or one, exactly one, or bounded list), missing-row behavior, and relevant `NULL` and join semantics.
- Apply least privilege to application database roles and enforce tenant or user isolation consistently across queries, joins, and aggregates.
- Test authorization and data isolation with the actual application database role, never only with a superuser, table owner, or privilege-bypassing role.
- Follow [security.md](./security.md) for production credentials and production-access approval.
  Never create a separate approval flow for database access.

## Default: tooling, ownership, and projections

- Reuse suitable existing migration and query tooling from the Project's stack instead of writing a custom migration runner.
- Keep SQL ownership clear inside existing domain module boundaries.
  Do not require an ORM, repository pattern, or wrapper abstraction when direct queries are clearer.
- Prefer explicit column lists and explicit aliases for application result contracts instead of wildcard projections.

## Mandatory: type and data contracts

- Define parameter and result contracts explicitly, including nullability, exact numeric precision, large integers, temporal types, JSON structures, and driver conversions.
- Provide real evidence that SQL, database schema, and application types agree.
  A handwritten generic parameter or type assertion is not evidence.
- Enforce suitable data invariants in the database with primary keys, foreign keys, uniqueness constraints, `NOT NULL`, and check constraints.
- Follow the Project's conventions in `<database docs path>` or ADRs for naming, key choices, numeric precision, and temporal values.

## Default: choosing contract proof

Choose contract-verification mechanisms suited to the Project's stack and record each mechanism's limits in `<database docs path>`:

- **Schema-derived or query-derived types**: prove static column and parameter shape against a captured schema, not runtime logic or live drift unless kept fresh in CI.
- **Database-backed SQL checking**: proves syntax and types against a schema, not concurrency or business invariants.
- **Boundary validation**: catches driver conversion and payload mismatches at runtime on executed paths, not dead branches.
- **Real-engine integration tests**: prove execution, constraints, driver conversions, and semantics on tested inputs, not untested branches.

## Mandatory: performance and data lifecycle

- Justify new or changed indexes with query plans and representative workloads.
  Weigh existing indexes, write cost, storage cost, and index-build lock impact before adding an index.
- Do not index every foreign key blindly.
  Index referencing columns when parent deletes, parent updates, or join lookups justify the write and storage cost.
- Bound result sizes on list queries and use deterministic ordering for pagination.
- Choose deletion, archival, retention, and soft-delete behavior from domain and compliance requirements.
  Data retention and deletion policy is a Decision for the Owner; see [review-and-merge.md](./review-and-merge.md#mandatory-decisions).
- Distinguish instants, calendar dates, local wall-clock times with time zones, and recurring schedules.
  Do not assume every date or time belongs in UTC, or that a database timestamp-with-time-zone type preserves the original time zone.

## Default: query efficiency and pagination

- Filter and aggregate in the database when it avoids transferring unnecessary rows or columns, and avoid repeated per-row queries.
- Prefer keyset pagination for large sequential traversal; allow bounded offset pagination when small datasets, shallow depth, or direct page jumps make it suitable.
- Verify query plans for exact row counts instead of assuming an index makes exact counts cheap under concurrent writes.

## Mandatory: downstream verification

Run SQL verification on disposable local instances of the actual target database engine using synthetic fixtures.
Never test against production data or substitute a different engine when engine behavior matters.
Preserve the separation between application tests and operational rehearsals in [testing.md](./testing.md) and [test-scope](../../checks/test-scope.md):

- **Application integration tests** (in declared application test paths, inside Feedback budgets): prove query and result contracts, relevant database constraints, least-privilege and tenant-isolation behavior under the actual application role, and deterministic transaction concurrency or retry behavior.
- **Migration verification** (via [migration-apply](../../checks/migration-apply.md), run on migration or schema changes): prove clean database creation from an empty database, upgrades from supported existing schema baselines, and data preservation with constraints intact.
- **Operational rehearsals** (in the separate non-application rehearsal path, outside routine application Feedback loops, required where risk warrants): prove tested restore procedures and recovery limits, interrupted and resumed bounded backfills, concurrent writes during multi-phase rollouts, and lock or duration impact on representative synthetic datasets.

When parallel agents run database tests, give each session its own disposable database or schema; see [delegation.md](./delegation.md#mandatory-isolated-checkouts).
