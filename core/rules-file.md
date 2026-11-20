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

Keep `.rules` under 32,000 bytes.
Some agents cut project instructions off near that size, and an agent that does not see a rule cannot follow it.
The same budget applies to each page that `.rules` tells agents to always read.
The check spec [doc-size](../checks/doc-size.md) enforces it.

When a page grows past the budget, split it by trigger or move detail to reference pages.
Never raise the budget.

## What belongs in it

The rules file holds:

- universal constraints that apply to every task in the Project;
- the reading map: which task page to read for which kind of task;
- the Review mode and the Kit version;
- the Agent skills block, which tells the pinned skills where the tracker, the triage labels, and the domain docs live.

The rules file does not hold:

- feature-specific rules; those go in the task page or the module's own `HOW-TO.md`;
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

`.rules` is the one rules file; <agent entry files> are links to it; edit `.rules` only.
Keep `.rules` and every always-read page under 32,000 bytes.
Skills live in `<skills folder>`; <other skills folders> link to it.

## Authority and reading

**Mandatory** instructions must be followed, including their stated exceptions.
**Defaults** apply unless a concrete reason justifies a departure; state that reason.
Unlabelled requirements in the pages below are mandatory; examples are not extra requirements.

Each kind of guidance has one owner:

- `.rules`: universal constraints and the reading map.
- `docs/agents/`: task-specific agent instructions.
- `HOW-TO.md` pages: current call contracts of a module.
  A HOW-TO beats an older ADR on call shape.
- ADRs in `docs/adr/`: decisions and their reasons.
  A newer accepted ADR that names what it supersedes beats the older one.
- `GLOSSARY.md` glossaries: domain language.

If code and a document disagree, find which one is stale; do not copy the code or pick the convenient rule.
Surface a conflict you cannot resolve; do not invent policy.

Instructions load every time; reference loads on a trigger.
Read a reference page when you are about to take the action that triggers it, not before.
Read a large reference page one section at a time.

| Task | Always read | Read when you are about to |
| ---- | ----------- | -------------------------- |
| Plan, spec, or split work, design a change, or build or prove a Ticket | `docs/agents/planning.md` | `docs/agents/issue-tracker.md` before you publish a Spec, a Ticket, or a Map |
| Write or review application code, config, or dependencies | `docs/agents/engineering.md` | <stack page> before you add, remove, or upgrade a dependency |
| Implement behavior, fix a bug, run checks, or defer tests | `docs/agents/testing.md` | `docs/agents/verification.md` before you change a user-visible feature |
| Commit, push, open or merge a pull request | `docs/agents/git.md`, `docs/agents/review-and-merge.md` | |
| Delegate, review subagent work, or hand work off | `docs/agents/delegation.md` | |
| Write docs, ADRs, or glossary terms | `docs/agents/documentation.md` | `docs/agents/domain.md` before you add or change a term |
| Create or update issues or pull requests | `docs/agents/issue-tracker.md` | |
| Handle a repeated agent mistake or an escaped defect | `docs/agents/corrections.md` | |
| Touch secrets, credentials, production, or untrusted input | `docs/agents/security.md` | |
| <Project-specific task> | <page> | <page> |

Read a module's `GLOSSARY.md` and `HOW-TO.md` before you edit an implementation file in its folder.
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

- Never skip Git hooks, by flag, environment variable, or configuration.
  Fix a hook failure; a request to finish quickly does not waive hooks.
- Start a persistent server, worker, or watcher only with the Owner's permission.
  Use an existing one when available.
  Short-lived servers that automated tests own and stop are allowed.
- Before the first task of a session, update the main checkout: fetch, then fast-forward pull when the branch tracks a remote.
  When the working tree is dirty or the branch has diverged, leave it and report why.
- Treat your training data as stale for every version-specific fact outside this repository.
  Verify each such fact as `docs/agents/engineering.md` says before you write code on it or recommend it.
- Pipe every command that prints JSON for you to read through `<JSON compactor command>`, because the compact form costs fewer tokens.
  Read raw JSON only when a program consumes it or you need its exact bytes.
  When the compactor fails, run the command again without it.
- Run <baseline test command> once before the first edit as the known-red baseline.
- Fix only the failures this change introduced.
  Stop after two failed fix attempts on one root cause; record it, report it, and continue.
- Work directly unless the Owner asks for delegation or invokes a skill whose workflow delegates.
- A requested Code review pauses integration and publishing until approval.
- Approval of a design is not permission to implement it.
- The Tier of a change comes from the tier check, never from the agent.
- Write no code comments outside the allow-list in `docs/checks/no-comments.md`.
  Put the reason for code in a name, an ADR, a test name, or the commit message.
- Never add agent attribution or co-author lines to commits, pull requests, issues, or comments.

## Communication

- Ask the Owner at most three questions per message; ask Decisions one at a time, each with a recommendation.
- Phrase each Decision for the Review mode: in mode Never, use no code and explain each new term in the same sentence.
- Show a literal sample (command, output, error line) instead of describing one.
````

## Fill-in rules

- Every command in the rules file must exist and work at Bootstrap time.
  Write UNKNOWN and open an issue when it does not.
- The reading map has one row per task page that the Project has.
  Add Project rows for each area that has its own page, such as deployment or a public SDK.
- Keep the boundary list short.
  Each boundary is a constraint that no task may break; anything narrower goes in a task page.
