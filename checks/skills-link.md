# skills-link

**Invariant**:
The Project has one skills folder.
Each other skills folder that an agent of the Project reads is a symbolic link to it.
Each skill in the folder is listed in the skills lock file or is a Project skill.

**Why**:
A skill that one agent sees and another does not makes agents behave differently.
An unpinned third-party skill can change without review.

**Applies when**: the Project has skills.

**Runs in**: CI.

**Failing case**:
A second skills folder is a real folder with its own copy of a skill, or a third-party skill is not in the lock file.

**Passing case**:
The second folder is a link to the first, and every third-party skill has a lock entry with a content hash.

**Error text**:
`<folder> must be a link to <skills folder>.` or `<skill> is not pinned in the skills lock file. Install it with the skills installer.`
