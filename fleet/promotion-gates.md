# Promotion gates

Agent autonomy grows in steps, from Gate 0 (assisted) through Gate 3 (steady state).
[core/agents/corrections.md](../core/agents/corrections.md#mandatory-gates-read-the-records) is the single home for the gate definitions, required merge counts, Review mode caps, and stop triggers.

## How a Fleet applies Project gates

- **Gates are per Project.**
  A Project that reached Gate 2 or Gate 3 never grants that gate to another Project.
  Every newly bootstrapped Project starts at Gate 0.
- **The Coordinator enforces each Project's active gate.**
  Include the Project's active gate in each Worker's Brief so Workers know whether they may land Tier 0 to 2 changes or stop at an open pull request.
- **Cross-Project stop triggers.**
  Any stop trigger in [core/agents/corrections.md](../core/agents/corrections.md#mandatory-stop-triggers) drops the affected Project to Gate 0 at once.
  When the failure class crosses Projects (such as a shared skill defect, credential misconfiguration, or leaked secret), pause agent merges across the whole Fleet until the Owner clears it.
- **No threshold drift.**
  Change a Project's gate thresholds only in that Project's ADR, never only in the Coordinator's notes.
