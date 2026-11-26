# doc-freshness

**Invariant**:
When a pull request changes a domain's declared public boundary files (exported entry points, routes or handlers, schemas, configuration definitions, or top-level module layout) and that domain owns a `HOW-TO.md`, `STATE.md`, or `MAP.md`, the pull request also updates an owning doc or states "No doc change: <reason>" in its Evidence bundle.
Internal refactors that do not touch declared public boundary files do not require a doc update or excuse.

**Why**:
Domain docs go stale when public contracts, limits, or topology change without updating their owning page.
Stale docs mislead later sessions.
Scoping this check to declared public boundary files prevents ceremonial doc churn on internal refactors.

**Applies when**: the Project has application code and domain docs (`HOW-TO.md`, `STATE.md`, or `MAP.md`) (Earned spec).

**Runs in**: CI, on pull request events.

**Failing case**:
A pull request adds a route or changes a schema in a domain's declared public boundary files, and that domain owns `HOW-TO.md`.
The pull request neither updates an owning doc (`HOW-TO.md`, `STATE.md`, or `MAP.md`) nor includes a "No doc change: <reason>" line in its Evidence bundle.

**Passing case**:
The same public boundary change updates the domain's `HOW-TO.md` or `STATE.md` (or `MAP.md` for a top-level layout change) in the same pull request.
Also passing: a pull request reformats an exported entry point without changing behavior, and its Evidence bundle states "No doc change: formatting only, public contract unchanged."
Also passing (false-positive trap): an internal refactor renames private helpers and restructures internal files inside the domain without touching any declared public boundary file, and includes neither a doc edit nor a "No doc change" line.

**Error text**:
`This pull request changes public boundary files in <domain> (<paths>), which owns <owning docs>, without updating those docs. Update the owning doc in this pull request, or state "No doc change: <reason>" in the Evidence bundle.`
