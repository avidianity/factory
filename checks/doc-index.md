# doc-index

**Invariant**:
Every active `docs/agents/*.md` task page, every domain folder doc (`GLOSSARY.md`, `HOW-TO.md`, `STATE.md`, `RULES.md`), and every topic guide under `how-to/` or `state/` is transitively reachable by links with read triggers starting from `.rules`.
For example, `.rules` links `MAP.md` or `GLOSSARY-MAP.md`, which links each domain folder, whose `HOW-TO.md` or `STATE.md` links its topic guides.
Files under `archive/`, `docs/adr/`, and `docs/research/` are exempt from `.rules` reachability, because agents open ADRs by slug and skip `archive/` unless asked about history.

**Why**:
When domain rules, how-tos, and current-state docs move out of `.rules` into domain folders, an unindexed file becomes invisible to agents.
Checking only local incoming links misses isolated islands where two orphan docs link to each other.
Verifying transitive reachability from `.rules` keeps every active doc discoverable without pre-loading it into every session.

**Applies when**: always.

**Runs in**: hook and CI.

**Failing case**:
A pull request adds `docs/domains/billing/STATE.md` or a topic guide `how-to/refunds.md`, and no link chain from `.rules` reaches it.
Also failing: two unreferenced topic guides link to each other, forming an isolated island unreachable from `.rules`.

**Passing case**:
`.rules` links `MAP.md`, `MAP.md` links the `billing` domain folder with a trigger, and `billing/HOW-TO.md` links `how-to/refunds.md` in its topic table with what change triggers reading it.
Also passing: a new ADR in `docs/adr/` is not linked from `.rules`.

**Error text**:
`<file> is not transitively reachable from .rules. Link it with a read trigger from .rules, MAP.md, GLOSSARY-MAP.md, or a reachable parent HOW-TO.md / STATE.md / docs/agents/ page, or move obsolete docs to archive/.`
