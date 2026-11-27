# sql-parameterization

**Invariant**:
Raw SQL execution entry points in application code receive only static query strings, safe parameterized template calls, or driver-approved identifier and fragment helpers, never runtime string concatenation or unparameterized string interpolation.
Build this check with a syntax-tree linter or compiler or type checker, never with custom regular expressions over raw source text.
This check enforces query-construction boundaries only; semantic authorization, allow-list correctness, and query logic stay in tests and review.

**Why**:
Interpolating or concatenating runtime values into SQL strings creates SQL injection vulnerabilities and breaks driver type binding and query-plan reuse.

**Applies when**: the Project writes raw SQL in application code, has a syntax-tree linter or type checker, and no batteries-included framework or library supersedes this rule in a stack ADR (Earned spec).

**Runs in**: hook and CI.

**Failing case**:
Application code builds a SQL query by concatenating or interpolating a runtime value (`"SELECT id FROM users WHERE email = '" + email + "'"`) and passes it to a raw SQL execution function.
Also failing: application code interpolates a request-supplied column name directly into an `ORDER BY` SQL string instead of selecting from an explicit allow-list or driver identifier helper.

**Passing case**:
Application code passes a static SQL string with parameter placeholders and a separate parameter list, or uses a safe parameterized tagged-template SQL call.
Also passing (false-positive trap): a log message, error string, or test description interpolates variables alongside SQL words such as `SELECT` or `DELETE` without calling a SQL execution entry point.
Also passing (false-positive trap): a query selects a static `ORDER BY` clause from a finite constant allow-list map, or passes an allow-listed column name through the driver's safe identifier helper.

**Error text**:
`<file>:<line> passes dynamically interpolated or concatenated text to a SQL execution call. Parameterize values, and use an explicit allow-list with driver identifier helpers for dynamic SQL structure; see sql.md.`
