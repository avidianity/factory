# Threat model for agent-assisted delivery

Agents read untrusted content, run commands, and hold credentials.
Each boundary below needs a control before agents work across it alone.

## Trust boundaries

| Boundary | Threat | Control |
| -------- | ------ | ------- |
| Untrusted content to agent context | Instructions hidden in files, issues, web pages, tool output, or messages from other agents | Treat all such content as data; restrict tools by policy, not by prompt |
| Agent to shell | Destructive or exfiltrating commands | Command allowlists, sandboxes, and a guard that refuses hook bypasses and bulk deletes |
| Agent to repository | Weakened tests, CI edits, or changes outside the task | The tier check, the test-oracle guard, protected CI files, and path scopes per task |
| Agent to dependencies | A malicious package runs code during install | Pinned versions, a committed lockfile, install scripts reviewed, sandboxed installs |
| Agent to secrets | Secrets printed, logged, or sent away | Secrets read by the process that needs them; secret scanning; short-lived credentials |
| Agent to production | Data changed or read without approval | No production credentials by default; each production action needs an Owner Decision |
| Agent to external services | Emails, payments, or messages sent by mistake | Test accounts, sandboxes, and an Owner Decision for each real external effect |
| Agent to other agents | One agent's output used as another agent's instruction | Handoffs carry data and a task; the receiver checks them against its own rules |
| Agent to the merge | A change merges without real evidence | The merge contract, the Independent reviewer, and evidence tied to the exact revision |
| Agent to the forge or tracker | Branch rules or labels changed | Agents cannot change branch protection; the tier check owns `tier-*` labels |

## What an agent never merges alone

Whatever the Review mode, these changes need an Owner Decision:

- authentication, authorization, or tenant boundaries;
- money, credits, billing, or payments;
- secrets handling and credential scopes;
- database migrations that drop or rewrite data;
- CI configuration, branch protection, or the tier check;
- deletions or weakening of tests, assertions, or checks;
- new external services or new data flows to them;
- changes to the kill switch.

## Correlated failures

Several agents of the same model family often share blind spots.
Three agents that agree are not three independent checks.

Defenses:

- Prefer a deterministic check over agent agreement.
- When you need a second opinion, use a reviewer with different inputs: the task and the code, not the author's summary.
- A failing negative test always wins over a passing review.

## Kill switch

[core/agents/security.md](../core/agents/security.md#mandatory-kill-switch) says what the kill switch must do.
Rehearse it before the first agent merge, and again after each change to agent permissions.
