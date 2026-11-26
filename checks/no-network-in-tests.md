# no-network-in-tests

**Invariant**:
Automated tests make no network connection beyond the local machine.

**Why**:
Network calls make tests slow, flaky, costly, and able to change real systems.

**Applies when**: the Project has automated tests (Earned spec).

**Runs in**: the test runner's setup, in every run.

**Failing case**:
A test calls a real external API.

**Passing case**:
The test injects a deterministic adapter, and the guard refuses any non-local connection with an error.

**Error text**:
`Test <name> tried to connect to <host kind>. Tests use injected adapters; no network beyond the local machine.`
