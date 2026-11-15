# agent-entry-links

**Invariant**:
Each agent entry file that [adapters/agents.md](../adapters/agents.md) lists for the Project's agents is a symbolic link to `.rules`, and `.rules` exists.

**Why**:
Two copies of the rules drift apart, and one agent then follows old rules.

**Applies when**: always.

**Runs in**: hook and CI.

**Failing case**:
An agent entry file is a regular file with its own text, or a link to a missing file.

**Passing case**:
Every listed entry file is a link whose target is `.rules`.

**Error text**:
`<file> must be a link to .rules. Edit .rules only, then recreate the link.`
