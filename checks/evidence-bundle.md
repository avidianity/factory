# evidence-bundle

**Invariant**:
Every pull request body has the sections of the Evidence bundle template: Tier, Task, Fail-first test, Checks, Safety proof, Runtime evidence, and Gaps.
Each section has content, or the words "Not applicable" with a reason.

**Why**:
Merge decisions that rest on missing evidence let unverified changes through.

**Applies when**: always.

**Runs in**: CI, on pull request events.

**Failing case**:
A body with only a summary paragraph.

**Passing case**:
A body that follows the template, with "Runtime evidence: Not applicable, no user-visible change."

**Error text**:
`The pull request body lacks <section>. Use templates/pr-evidence.md.`
