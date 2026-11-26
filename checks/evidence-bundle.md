# evidence-bundle

**Invariant**:
Every pull request body has the sections of the Evidence bundle template: Tier, Task, Fail-first test, Checks, Safety proof, Runtime evidence, and Gaps.
Each section has content, or the words "Not applicable" with a reason.
The check verifies that the revision cited in the Evidence bundle matches the pull request's exact head commit SHA.
Forge required status checks are the authoritative proof for CI checks, never pull request prose alone.
For Tier 2 and above, Safety proofs and user-visible runtime evidence reference machine-generated CI or verifier artifacts stamped with that exact head commit SHA.
Fail-first test proof stays lightweight in Day-1 CI (pull request output, re-verified by an Independent reviewer at Tier 3) rather than forcing double-checkout CI replay.

**Why**:
Merge decisions that rest on missing, stale, or prose-only evidence let unverified changes through.
Pinning evidence to the exact head commit SHA and backing Tier 2+ proofs with CI or verifier artifacts stops fabricated or outdated claims without slowing down Day-1 CI.

**Applies when**: always.

**Runs in**: CI, on pull request events.

**Failing case**:
A body with only a summary paragraph.
Also failing: the Evidence bundle cites an earlier commit SHA than the pull request head, or a Tier 2+ user-visible change offers prose-only runtime claims with no revision-stamped CI or verifier artifact.

**Passing case**:
A body that follows the template, matches the exact pull request head commit SHA, passes required CI status checks on that revision, and links revision-stamped artifacts for Tier 2+ Safety proofs or runtime evidence (or states "Runtime evidence: Not applicable, no user-visible change.").

**Error text**:
`The pull request Evidence bundle lacks <section>, cites stale revision <sha> instead of head <head_sha>, or lacks a revision-stamped artifact for <proof>. Use templates/pr-evidence.md.`
