# secret-scan

**Invariant**:
No commit adds text that matches a known secret format or a high-entropy token assigned to a name such as key, token, secret, or password.

**Why**:
A secret in Git history stays there after a later commit removes it.

**Applies when**: always.

**Runs in**: hook and CI.

**Failing case**:
A config file with `API_KEY=` followed by a 40-character random string.

**Passing case**:
The config file reads the value from the environment, and the example file has an empty value.

**Error text**:
`<file>:<line> looks like a secret. Remove it, rotate it if it was real, and read it from the secret store.`
