# doc-size

**Invariant**:
Every active documentation and guidance file stays under its tiered byte budget:

- `.rules`: under 8,000 bytes.
- Each `docs/agents/*.md` task page and each domain `RULES.md`: under 16,000 bytes.
- Every other active documentation file (`MAP.md`, `GLOSSARY.md`, `GLOSSARY-MAP.md`, `HOW-TO.md`, `STATE.md`, `how-to/*.md`, `state/*.md`, `docs/domains/**/*.md`): under 32,000 bytes.

Files under an `archive/` folder are exempt.

**Why**:
Agents load `.rules` on every session, so a large rules file wastes context before real work starts.
Oversized task pages and monolith reference docs also waste context or get truncated; strict tiered budgets force generalized routing in `.rules` and index-plus-topic splits everywhere else.

**Applies when**: always.

**Runs in**: hook and CI.

**Failing case**:
`.rules` is 9,500 bytes.
Also failing: `docs/agents/testing.md` is 18,000 bytes, or `docs/domains/deployment/STATE.md` is 40,000 bytes.

**Passing case**:
`.rules` is 5,200 bytes, every `docs/agents/*.md` and domain `RULES.md` is under 16,000 bytes, and every other active doc is under 32,000 bytes.
Also passing: a historical file in `docs/adr/archive/` is 35,000 bytes.

**Error text**:
`<file> is <n> bytes; the budget for <tier> is <limit>. Move domain detail into a domain folder or split into an index and topic guides; never raise the budget.`
