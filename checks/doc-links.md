# doc-links

**Invariant**:
Every relative link in a Markdown file resolves to an existing file, and every heading anchor in a link exists in its target.

**Why**:
Agents follow links to find the page that owns a rule.
A broken link hides the rule.

**Applies when**: always.

**Runs in**: CI, and in the hook for changed Markdown files.

**Failing case**:
`docs/agents/git.md` links to `./review.md#tiers`, and that file or heading does not exist.

**Passing case**:
Every relative link and anchor resolves.

**Error text**:
`<file>:<line> links to <target>, which does not exist. Fix the link or restore the target.`
