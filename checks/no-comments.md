# no-comments

**Invariant**:
No line that a change adds to a source file holds a comment, except a comment on the allow-list.
The allow-list is:

- a shebang line;
- a license header that the Project's license requires;
- a directive that the language, compiler, or build tool reads, such as an encoding declaration or a build constraint;
- a doc comment on an exported symbol of a published package, when the package's API reference is generated from it.

Generated files are exempt.
A suppression directive is not on the allow-list; [no-suppressions](./no-suppressions.md) owns it.

**Why**:
A comment repeats a fact that the code, an ADR, a test, or the tracker already owns.
No check keeps it true, so it goes stale and misleads the next reader.
More text in each file also means more for agents to read and for checks to scan.

**Applies when**: the Project has application code.

**Runs in**: hook and CI, on the lines that the change adds.
Existing comments stay until their cleanup issue removes them.

**Allow-list file**:
The Project keeps the allow-list in `docs/checks/no-comments.md`.
A change to the allow-list is Tier 3; see [test-oracle-guard](./test-oracle-guard.md).

**Failing case**:
A change adds a comment that says what the next line does.
Also failing: a change adds a line of commented-out code, or a comment that marks work to do later.

**Passing case**:
The change renames the function so that the comment adds nothing, and deletes the comment.
Work to do later is a tracker issue.

**Error text**:
`<file>:<line> adds a comment. Delete it. Put the reason in a name, an ADR, a test name, the commit message, or a tracker issue; see engineering.md.`
