# doc-index

**Invariant**:
Every active `docs/agents/*.md` task page, every domain folder doc (`GLOSSARY.md`, `HOW-TO.md`, `STATE.md`, `RULES.md`), and every topic guide under `how-to/` or `state/` is reachable by a link with a read trigger from `.rules`, `MAP.md`, `GLOSSARY-MAP.md`, or its owning parent index (`HOW-TO.md`, `STATE.md`, or a `docs/agents/` task page).
Files under `archive/`, `docs/adr/`, and `docs/research/` are exempt from `.rules` routing, because agents open ADRs by slug and skip `archive/` unless asked about history.

**Why**:
When domain rules, how-tos, and current-state docs move out of `.rules` into domain folders, an unindexed file becomes invisible to agents.
Indexing every active file from its parent index with an explicit trigger keeps docs discoverable without pre-loading them into every session.

**Applies when**: always.

**Runs in**: hook and CI.

**Failing case**:
A pull request adds `docs/domains/billing/STATE.md` or a topic guide `how-to/refunds.md`, and neither `.rules`, `MAP.md`, nor the domain's `HOW-TO.md` links to it.

**Passing case**:
`MAP.md` links to the `billing` domain folder with a trigger, and `billing/HOW-TO.md` links `how-to/refunds.md` in its topic table with what change triggers reading it.
Also passing: a new ADR in `docs/adr/` is not listed in `.rules`.

**Error text**:
`<file> is not linked from an index (.rules, MAP.md, GLOSSARY-MAP.md, or its parent HOW-TO.md / STATE.md / docs/agents/ page). Add a link with a read trigger, or move obsolete docs to archive/.`
