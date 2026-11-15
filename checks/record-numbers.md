# record-numbers

**Invariant**:
Numbered records, such as ADRs and migrations, have unique numbers with no gaps in their sequence.

**Why**:
Two pull requests can each add record 0042 and each pass alone.
After both merge, links and tools that expect one record per number break.

**Applies when**: always.

**Runs in**: CI, and in the merged-result check before each merge.

**Failing case**:
`docs/adr/0042-a.md` and `docs/adr/0042-b.md` both exist.

**Passing case**:
Numbers run 0001, 0002, and so on, each used once.

**Error text**:
`Record number <n> is used by <file a> and <file b>. Renumber the newer one to <next free number>.`
