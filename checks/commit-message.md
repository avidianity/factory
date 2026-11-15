# commit-message

**Invariant**:
Each commit message follows Conventional Commits with an allowed scope.
It holds no co-author line, no agent attribution, and no marker that tells CI to skip the commit.

**Why**:
Attribution lines publish tool names under the Owner's identity.
A skip marker merges a commit that no check ran on.

**Applies when**: always.

**Runs in**: the commit-message hook, and CI for every commit in a pull request.

**Failing case**:
A message with a `Co-Authored-By:` trailer, or a subject without a type.

**Passing case**:
`fix(auth): refuse expired session tokens`

**Error text**:
`Commit message: <problem>. Use "<type>(<scope>): <summary>" with no co-author or attribution lines.`
