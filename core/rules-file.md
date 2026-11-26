# The Project rules file

Every Project has one rules file that every coding agent reads at the start of every session.
This page gives its template and the rules for keeping it small and correct.

## Where it lives

The rules file is `.rules` at the Project root.
Each agent's own entry file, such as `AGENTS.md`, is a symbolic link to `.rules`.
[adapters/agents.md](../adapters/agents.md) lists the entry file and skills folder of each agent.
The check spec [agent-entry-links](../checks/agent-entry-links.md) keeps the links correct.

Edit `.rules` only.
Never edit an agent entry file directly, because it is a link.

## Size

Keep `.rules` under 8,000 bytes.
Agents load `.rules` on every session, so every extra paragraph wastes context on every task.
Keep each `docs/agents/*.md` task page and each domain `RULES.md` under 16,000 bytes, and every other active documentation file under 32,000 bytes.
The check spec [doc-size](../checks/doc-size.md) enforces these tiered budgets, and [doc-index](../checks/doc-index.md) keeps every split file reachable from an index.

When a page nears its budget, move domain detail into its domain folder or split the page into an index and topic guides.
Never raise a budget.

## What belongs in it

The rules file holds generalized project-wide information only:

- project identity in one or two sentences;
- universal non-negotiable boundaries that apply to every task in the Project (one or two lines each);
- the routing map: which task page or domain doc to read for which trigger;
- the Review mode, the Standing Decisions ADR link, and the Kit version;
- the Agent skills block, which tells the pinned skills where the tracker, the triage labels, and the domain docs live.

The rules file never holds:

- domain-specific or feature-specific rules and boundary exceptions; those go in that domain's `RULES.md`;
- how-to contracts or multi-step procedures; those go in `docs/agents/` or the domain's `HOW-TO.md`;
- current-state facts about subsystems or deployments; those go in `MAP.md` or the domain's `STATE.md`;
- explanations, history, or examples; those go in ADRs and reference pages;
- personal preferences of one developer; those go in that developer's own agent config.

A rule that a check enforces still gets one line in the rules file, with a link to the check spec, so that agents know why the check fails.

## Template

Copy the block below into the Project's `.rules`.
Replace every placeholder in angle brackets.
Delete a line only when the Owner removed that rule, and record the removal in an ADR.

````markdown
# Agent guidance

<Project name> is <one sentence from the Owner's Purpose answer>.

Kit: avidianity/factory <Kit version>, bootstrapped <YYYY-MM-DD>.
Review mode: <Always | Critical only | Never> ([ADR <number>](docs/adr/<file>.md)).
Standing Decisions: <none yet | [ADR <number>](docs/adr/<file>.md)>.

`.rules` is the one rules file; <agent entry files> are links to it; edit `.rules` only.
Keep `.rules` under 8,000 bytes, task pages and domain `RULES.md` under 16,000 bytes, and all other active docs under 32,000 bytes.
Skills live in `<skills folder>`; <other skills folders> link to it.

## Authority and reading

**Mandatory** instructions must be followed, including their stated exceptions.
**Defaults** apply unless a concrete reason justifies a departure; state that reason.
Unlabelled requirements in the pages below are mandatory; examples are not extra requirements.

Each kind of guidance has one owner:

- `.rules`: generalized project boundaries and the routing map only.
- `docs/agents/`: cross-cutting task-specific agent instructions.
- Domain `RULES.md`: domain-specific agent constraints and boundary exceptions.
- Domain `HOW-TO.md` (and `how-to/`): current call contracts and how-to guides.
  A HOW-TO beats an older ADR on call shape.
- `MAP.md` and domain `STATE.md` (and `state/`): current system topology and current state of a domain.
- ADRs in `docs/adr/`: decisions and their reasons.
  A newer accepted ADR that names what it supersedes beats the older one.
- `GLOSSARY.md` glossaries: domain language.

If code and a document disagree, find which one is stale; do not copy the code or pick the convenient rule.
Surface a conflict you cannot resolve; do not invent policy.

Instructions load on task match; reference loads on a trigger.
Read only the task page(s) and files the current request needs, never unrelated pages for general context.
For a narrow operational request (such as committing, pushing, or running one command), read only its matching task page and skip unrelated docs, maps, and source files.

| Task | Always read | Read when you are about to |
| ---- | ----------- | -------------------------- |
| Plan, spec, or split work, design a change, or build or prove a Ticket | `docs/agents/planning.md` | `docs/agents/issue-tracker.md` before you publish a Spec, a Ticket, or a Map |
| Write or review application code, config, or dependencies | `docs/agents/engineering.md` | <stack page> before you add, remove, or upgrade a dependency; `MAP.md` before you cross domain boundaries |
| Implement behavior, fix a bug, run checks, or defer tests | `docs/agents/testing.md` | `docs/agents/verification.md` before you change a user-visible feature |
| Commit, push, open or merge a pull request | `docs/agents/git.md` | `docs/agents/review-and-merge.md` before you open or merge a pull request |
| Delegate, review subagent work, or hand work off | `docs/agents/delegation.md` | |
| Write or update rules (`.rules`, `docs/agents/`, domain `RULES.md`), docs, `MAP.md`, `STATE.md`, `HOW-TO.md`, ADRs, or glossary terms | `docs/agents/documentation.md` | `docs/agents/domain.md` before you add or change a term |
| Create or update issues or pull requests | `docs/agents/issue-tracker.md` | |
| Handle a repeated agent mistake or an escaped defect | `docs/agents/corrections.md` | |
| Touch secrets, credentials, production, or untrusted input | `docs/agents/security.md` | |
| <Project-specific task> | <page> | <page> |

Read a domain's or module's `RULES.md`, `GLOSSARY.md`, `HOW-TO.md`, and `STATE.md` (when present) before you edit files in its folder.
When `HOW-TO.md` or `STATE.md` indexes topic guides in `how-to/` or `state/`, read only the guides for the area you edit.
Open ADRs in `docs/adr/` by slug match only when you change or contradict a decision; skip `archive/` unless the task is about history.
Use glossary terms as written.
Find runnable commands in <manifest file> and hook commands in <hook config file>.

## Agent skills

### Issue tracker

Issues, Specs, Tickets, and Maps live in <tracker>.
See `docs/agents/issue-tracker.md`.

### Triage labels

Each triage role uses the label of the same name.
See `docs/agents/issue-tracker.md`.

### Domain docs

<Single-context: one `GLOSSARY.md` at the root | Multi-context: `GLOSSARY-MAP.md` at the root>, and ADRs in `docs/adr/`.
See `docs/agents/domain.md`.

## Mandatory boundaries

<Project-specific boundaries from the Owner's answers: data isolation, forbidden actions, production rules.>

- Never skip Git hooks; fix hook failures instead.
- Start a persistent server, worker, or watcher only with Owner permission; short-lived test-owned servers are allowed.
- Before the first code or branch task, fast-forward the main checkout when clean and tracking a remote; otherwise leave it and report why.
- Read and search only what the current request needs; stop searching once you have enough facts to act, and never bundle unrequested changes.
- Do not hunt for unrelated bugs; if you directly hit a real bug or risk, finish the requested task untouched and flag it in one short line at the end.
- Verify version-specific external facts per `docs/agents/engineering.md` before writing code or recommending them.
- Pipe JSON output for agent reading through `<JSON compactor command>`; fall back to raw JSON on failure or when a program consumes it.
- Run `<baseline test command>` once before the first edit, fix only introduced failures, and stop after two failed attempts on one root cause.
- Work directly unless the Owner asks for delegation or starts a delegating skill.
- A requested Code review pauses integration and publishing until approval; design approval is not permission to build.
- The Tier comes from the tier check on the protected base revision, never from the agent.
- Write no code comments outside the allow-list in `docs/checks/no-comments.md`.
- Add tests and collect coverage only on declared application paths in `docs/checks/test-scope.md`; never test or measure coverage on tool configs or low-risk helper scripts.
- Keep `.rules` under 8,000 bytes for generalized rules and routing only; read `docs/agents/documentation.md` before changing any rule or doc.
- Never add agent attribution or co-author lines to commits, pull requests, issues, or comments.

## Communication

- Ask the Owner at most three questions per message; ask Decisions one at a time, each with a recommendation.
- Phrase each Decision for the Review mode: in mode Never, use no code and explain each new term in the same sentence.
- Show a literal sample (command, output, error line) instead of describing one.
- When the Owner asks which skill to use or how to start one, answer from `docs/owner-guide.md`, and show the exact message to type.
````

## Fill-in rules

- Every command in the rules file must exist and work at Bootstrap time.
  Write UNKNOWN and open an issue when it does not.
- The reading map has one row per task page that the Project has.
  Add Project rows for each area that has its own page, such as deployment or a public SDK.
- Keep the boundary list short.
  Each boundary is a constraint that no task may break; anything narrower goes in a task page.
