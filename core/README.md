# Core

Core holds the stack-free rules that every Project receives.

- [rules-file.md](./rules-file.md): the template for the Project's one rules file.
- [owner-guide.md](./owner-guide.md): the template for the Project's Owner guide, the tutorial that tells the Owner which skill to start and what to type.
- [agents/](./agents/): task pages that Bootstrap copies to the Project's `docs/agents/`, with Project commands and paths filled in (conditional pages only when applicable).

| Page | Owns |
| ---- | ---- |
| [engineering.md](./agents/engineering.md) | External facts, dependencies, configuration, errors, stack-convention conflicts |
| [sql.md](./agents/sql.md) | SQL schema ownership, migrations, rollouts, recovery, concurrency, query safety, contracts, performance, and verification (copied when the Project uses SQL and no batteries-included framework or library supersedes it) |
| [testing.md](./agents/testing.md) | Behavior tests, the check ladder, the known-red baseline, failure classes, the stopping rule, flaky tests |
| [git.md](./agents/git.md) | Authorization to commit and publish, branches, commits, the CI gate, the merged-result check, stacks |
| [review-and-merge.md](./agents/review-and-merge.md) | Tiers, Review modes, Decisions, the merge contract, the Evidence bundle, the Risk summary |
| [planning.md](./agents/planning.md) | The pipeline from idea to proof: Specs, Tickets, Design sketches, rigor by Expected Tier, proof that runs |
| [delegation.md](./agents/delegation.md) | Subagents inside one Project: fresh sessions, isolated checkouts, exact handoffs, integration |
| [documentation.md](./agents/documentation.md) | One home per fact, HOW-TO, ADR, research, size budget, writing style |
| [domain.md](./agents/domain.md) | Glossaries and the context map |
| [issue-tracker.md](./agents/issue-tracker.md) | What goes in the tracker, labels, owner labels, stale specs |
| [verification.md](./agents/verification.md) | The Verification contract, Feature maps, and keeping them current |
| [corrections.md](./agents/corrections.md) | Corrections, the decision order, Escaped defects, and records that gates read |
| [security.md](./agents/security.md) | Forbidden actions, untrusted content, secrets, least privilege, kill switches |

Each page marks its sections **Mandatory** or **Default**.
A Project may change a Default with a reason in an ADR.
A Project may change a Mandatory rule only by an Owner Decision recorded in an ADR.
