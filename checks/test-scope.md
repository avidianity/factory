# test-scope

**Invariant**:
Every test file in the application test suite lives under the Project's declared application test paths.
No test file in the application test suite targets a repository root config file, a declarative tool config file, or a low-churn repository script.
Automated self-tests for custom check scripts live in a separate check-test path that runs only when check scripts change, never inside the application Feedback loops.
When the Project uses coverage, complexity-coverage, or mutation tools, their configuration collects only an explicit allow-list inside the declared application source paths and excludes config files, repository scripts, generated files, migrations, type-only files, thin entry points, and test helpers.

**Why**:
Agents add unit tests for static config files and one-off scripts whenever they touch them.
Unscoped coverage tools also report zero percent on root config files and scripts, which pressures agents into writing filler tests to satisfy floors.
Those tests mirror config values without proving user behavior, slow down every local and CI Feedback loop, and stay locked behind [test-oracle-guard](./test-oracle-guard.md).

**Applies when**: the Project has application code.

**Runs in**: hook and CI.

**Scope configuration**:
The Project keeps its declared application source paths, application test paths, coverage exclusions, and optional check-test path in one file that this check, the test runner, and any coverage tool read.
A change to that file is Tier 3; see [test-oracle-guard](./test-oracle-guard.md).

**Failing case**:
A pull request adds a test file at the repository root beside a commit-message config file or a linter config file.
Also failing: a pull request adds a test for a build config file or a one-off developer script inside the application test suite.
Also failing: the Project's coverage configuration collects repository-wide without an application source allow-list, or includes root config files, tooling configs, or repository scripts.

**Passing case**:
A pull request adds a behavior test under the declared application test paths.
Also passing: a pull request adds a self-test for a custom policy check script under the separate check-test path, wired to run only when check scripts change.
Also passing: the Project's coverage configuration uses an explicit allow-list inside declared application source paths and excludes configs, scripts, generated files, migrations, type-only files, thin entry points, and test helpers.

**Error text**:
`<file> is outside the declared application test paths. Do not add automated tests for declarative tool configs or low-churn scripts; put custom check-script tests in <check-test path>. See testing.md.`
`Coverage or mutation config <file> includes non-application paths (<paths>). Collect only declared application source paths and exclude configs, scripts, generated files, migrations, type-only files, and test helpers; see testing.md.`
