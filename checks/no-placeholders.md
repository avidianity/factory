# no-placeholders

**Invariant**:
No file in `.rules`, `docs/owner-guide.md`, `docs/agents/`, or `docs/checks/` holds a Kit placeholder: text in angle brackets that starts with a lowercase word, such as `<main branch>`.
Code samples inside fenced blocks are exempt only when the block is marked as a template.

**Why**:
A placeholder left by Bootstrap tells an agent to use a value that does not exist.

**Applies when**: always.

**Runs in**: CI.

**Failing case**:
`docs/agents/git.md` contains `Allowed scopes: <scope list>.`

**Passing case**:
Every placeholder is replaced with a Project value or the word UNKNOWN with a linked issue.

**Error text**:
`<file>:<line> still holds the placeholder <text>. Replace it with the Project value.`
