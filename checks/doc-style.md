# doc-style

**Invariant**:
Markdown files contain no em dash character (U+2014), and pass the Project's Markdown linter.

**Why**:
The Project writes in one plain style, and the linter keeps structure that agents rely on, such as heading levels.

**Applies when**: always.

**Runs in**: hook and CI.

**Failing case**:
A line in `docs/agents/testing.md` contains an em dash.

**Passing case**:
The line uses a plain dash or two sentences.

**Error text**:
`<file>:<line> uses an em dash. Use a plain dash "-" or split the sentence.`
