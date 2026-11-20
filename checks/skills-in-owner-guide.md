# skills-in-owner-guide

**Invariant**:
The name of each skill in the Project's skills folder appears in `docs/owner-guide.md`.

**Why**:
The Owner starts most skills.
A skill that the Owner guide does not name is a skill the Owner cannot find, or starts without knowing what it expects.

**Applies when**: the Project has skills.

**Runs in**: CI.

**Failing case**:
The skills folder holds `release-notes/SKILL.md`, and `docs/owner-guide.md` does not contain `release-notes`.

**Passing case**:
`docs/owner-guide.md` has a "Pick a skill" row for `release-notes`, or lists it under "Skills that you do not start".

**Error text**:
`<skill> is not in docs/owner-guide.md. Add when the Owner starts it, what to type, and what comes back, or list it as a helper.`
