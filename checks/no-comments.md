# no-comments

**Invariant**:
No line that a change adds to a source file holds a comment, except a comment on the allow-list.
The allow-list is:

- a shebang line;
- a license header that the Project's license requires;
- a directive that the language, compiler, or build tool reads, such as an encoding declaration or a build constraint;
- a doc comment on an exported symbol of a published package, when the package's API reference is generated from it;
- a tagged constraint comment that explains a non-obvious technical constraint that names, types, or a test cannot express, prefixed with `INVARIANT: <why>`, `SAFETY: <why>`, or `WORKAROUND(<ref>): <why>` (where `<ref>` names an issue, ADR, or URL).

Generated files are exempt.
A suppression directive is not on the allow-list; [no-suppressions](./no-suppressions.md) owns it.
Build this check with a syntax-tree parser or linter, never regular expressions over raw source text.

**Why**:
Narration comments repeat facts that code, ADRs, tests, or the tracker already own, and go stale without a check.
At the same time, non-obvious local constraints (such as ordering invariants, security timing rules, or upstream workarounds) can be deleted by later edits when invisible at the line of code.
Machine-checked constraint tags block comment noise while keeping critical constraints visible.

**Applies when**: the Project has application code and a syntax-tree linter or parser (Earned spec).

**Runs in**: hook and CI, on the lines that the change adds.
Existing comments stay until their cleanup issue removes them.

**Allow-list file**:
The Project keeps the allow-list in `docs/checks/no-comments.md`.
A change to the allow-list is Tier 3; see [test-oracle-guard](./test-oracle-guard.md).

**Failing case**:
A change adds a bare comment that says what the next line does.
Also failing: a change adds commented-out code, a work-to-do comment, or a `WORKAROUND:` comment with no issue, ADR, or URL reference.

**Passing case**:
The change renames the function so that a narration comment adds nothing, and deletes the comment.
Work to do later is a tracker issue.
Also passing: a line holds a string literal containing `//` or `/*`, such as a URL.
Also passing: a line holds `WORKAROUND(#412): flush buffer before close to avoid truncated writes on older runtimes`.

**Error text**:
`<file>:<line> adds an untagged comment. Delete it, put the reason in a name, ADR, test, commit message, or issue, or tag a non-obvious constraint with INVARIANT:, SAFETY:, or WORKAROUND(<ref>):; see engineering.md.`
