# tool-servers-render

**Invariant**:
Agent tool servers (for example MCP servers) are listed once in one source file.
Each agent's own tool-server config file is generated from that source, and the committed file equals the generated output.

**Why**:
Hand-kept config files per agent drift, and then one agent lacks a tool or uses an old address.

**Applies when**: the Project configures agent tool servers.

**Runs in**: CI.

**Failing case**:
Someone edits one agent's config file by hand, so it differs from the generated output.

**Passing case**:
The generator runs, and no committed file changes.

**Error text**:
`<file> differs from the tool-server source. Edit <source file> and run <generate command>.`
