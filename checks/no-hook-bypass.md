# no-hook-bypass

**Invariant**:
Every check that a hook runs also runs in CI on the same files.
Agent sessions refuse commands that skip hooks, such as a no-verify flag or a hooks-path override.

**Why**:
A hook that someone can skip is advice, not a check.

**Applies when**: always.

**Runs in**: CI for the parity part; the agent's command guard for the refusal part, where the agent supports one ([adapters/agents.md](../adapters/agents.md)).

**Failing case**:
A commit that a skipped hook would have rejected reaches the pull request, and CI does not run the same check.

**Passing case**:
CI runs the same check and fails the pull request.

**Error text**:
`<check> failed in CI. A local hook should have caught this. Never skip hooks; fix the failure and commit again.`
