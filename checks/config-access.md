# config-access

**Invariant**:
Only the Project's configuration modules read environment variables or other process configuration.

**Why**:
Configuration read all over the code skips validation and hides which settings exist.

**Applies when**: the Project has application code.

**Runs in**: hook and CI, as a lint or source rule.

**Failing case**:
A request handler reads an environment variable directly.

**Passing case**:
The handler receives a typed value from the configuration module.

**Error text**:
`<file>:<line> reads configuration directly. Read it in <config module> and pass the typed value.`
