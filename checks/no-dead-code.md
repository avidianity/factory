# no-dead-code

**Invariant**:
A pull request adds no dead symbol.
A dead symbol is a file, function, type, constant, export, or dependency that nothing reachable from a Project entry point uses.
The exported API of a published package is an entry point.
A symbol in application code that only tests use is dead.

The check compares the dead symbols on the pull request head with those on the base branch.
It fails when the head has a dead symbol that the base branch lacks, wherever that symbol is.

**Why**:
Dead code costs build time, test time, and reading time, and returns nothing.
A change often kills code in a file that it does not touch, by deleting the last caller.
A check that reads only the changed lines misses that.

**Applies when**: the Project has application code.

**Runs in**: CI.

**Failing case**:
A pull request deletes the last caller of a function in another file, and leaves the function.

**Passing case**:
The pull request deletes the function too.
Dead symbols that existed on the base branch do not fail the check; their cleanup issue removes them.

**Error text**:
`<symbol> in <file> is dead after this change: nothing reachable from an entry point uses it. Delete it.`
