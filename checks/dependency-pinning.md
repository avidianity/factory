# dependency-pinning

**Invariant**:
Every direct dependency has an exact version, and the lockfile is committed and matches the manifest.

**Why**:
A version range can install new code that nobody reviewed.

**Applies when**: the Project has dependencies.

**Runs in**: hook and CI.

**Failing case**:
The manifest asks for any version compatible with 2.1, or the lockfile is out of date.

**Passing case**:
The manifest names version 2.1.4, and an install from the lockfile changes nothing.

**Error text**:
`<dependency> is not pinned to an exact version, or the lockfile is stale. Pin it and update the lockfile.`
