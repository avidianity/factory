# behavior-has-test

**Invariant**:
A pull request that changes files under the Project's declared application source paths also changes or adds a test file under the declared application test paths, or its body says "No behavior change" with a reason.
Changes outside declared application source paths, including root or tooling configuration files, repository and development scripts, generated files, and documentation, do not require a test file.

**Why**:
A behavior change without a test can break again without anyone seeing it.
Scoping this check strictly to application source paths stops agents from adding low-value config or script tests just to pass CI.

**Applies when**: the Project has application code.

**Runs in**: CI, on pull request events.

**Failing case**:
A pull request changes a file under the declared application source paths and no test file, and its body has no "No behavior change" line.

**Passing case**:
The same change also adds a test under the declared application test paths, or it is a rename with "No behavior change: rename only."
Also passing: a pull request edits a commit-message config file, a linter config file, or a repository script with no test file added.

**Error text**:
`This pull request changes application source files under <app source paths> but no tests. Add a fail-first test, or state "No behavior change" with a reason.`
