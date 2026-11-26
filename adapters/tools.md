# Tools adapter

This page names tools that fit the Kit's neutral roles.
Each tool is an example; use the Project's or the Owner's own tool when it fills the role.

Last checked: 2026-11-19.

## Isolated checkouts

Role: give each agent its own checkout of the repository.

| Tool | How |
| ---- | --- |
| Git worktrees | `git worktree add <path> -b <branch> <base commit>`; remove with `git worktree remove <path>` |
| Treehouse | A pool of pre-warmed worktrees with durable leases. `treehouse get --lease --lease-holder <label> --json` returns a path and a lease id; `treehouse return --if-lease-id <id> <path>` returns it. Run the return from outside the leased checkout. |

Treehouse may start a lease on a newer remote tip than you asked for.
After you get a lease, check out the exact baseline commit and verify `HEAD`.

## Skills installer

Role: install third-party skills and pin them in a lock file.

| Tool | How |
| ---- | --- |
| `skills` CLI (npm package `skills`, version 1.9.0 checked) | `npx skills add <owner>/<repo> --skill <name> --agent <agent>` installs a skill and records it in `skills-lock.json` with a content hash. `npx skills experimental_install --frozen-lockfile` restores skills from the lock file and fails when the lock file would change. |

Check the installed version's `--help` before you use a flag; the restore command is marked experimental.

Install the Kit's pinned skills, from the Project root:

```text
npx skills@1.9.0 add mattpocock/skills --skill grill-me --skill grilling --skill wait-what --skill grill-with-docs --skill domain-modeling --skill wayfinder --skill research --skill prototype --skill triage --skill to-spec --skill to-tickets --skill implement --skill implement-spec --skill tdd --skill codebase-design --skill code-review --agent codex -y
npx skills@1.9.0 add backnotprop/pstack --skill architect --skill arena --skill sharpen --skill blast-radius --skill interrogate --skill in-your-own-words --skill how --skill why --skill unslop --skill show-me-your-work --skill principle-separate-before-serializing-shared-state --skill principle-redesign-from-first-principles --skill principle-prove-it-works --skill principle-laziness-protocol --skill principle-encode-lessons-in-structure --agent codex -y
```

`--agent codex` installs into `.agents/skills`, the Kit's one skills folder.
The other agents read it through the links in [agents.md](./agents.md#the-one-source-layout).

Restore them on a new machine, or in CI:

```text
npx skills@1.9.0 experimental_install --frozen-lockfile --agent codex
```

If `experimental_install` breaks or a remote source is unavailable, follow the pinned-commit fallback in [skills/README.md](../skills/README.md#fallback-when-the-installer-or-mirror-fails).

## JSON compactor

Role: convert JSON that a command prints into a compact text form, so that an agent reads it with fewer tokens.

| Tool | How |
| ---- | --- |
| TOON CLI (npm package `@toon-format/cli`, version 4.4.0 checked) | Install with `npm install -g @toon-format/cli@4.4.0`. Pipe output into it: `<command> \| toon`. It detects JSON input and prints TOON. `toon --stats` also prints the token counts of both forms. |

The TOON CLI exits with status 1 and prints `Failed to parse JSON` when its input is not JSON.

## Terminal multiplexers

Role: run many agent sessions side by side, and let a coordinator read and send to them.
Only the Fleet uses this role.

| Tool | Notes |
| ---- | ----- |
| tmux | Sessions, windows, and panes; `send-keys` and `capture-pane` let a script read and type. |
| cmux | A terminal app for agent sessions, with workspaces and surfaces that scripts can address. |

Prefer the agents' own messaging between sessions over typing into a terminal, because typed text can mix with a person's input.

## Markdown and link checks

Role: the [doc-style](../checks/doc-style.md) and [doc-links](../checks/doc-links.md) checks.

| Tool | Notes |
| ---- | ----- |
| markdownlint-cli2 | Markdown linter; the Kit's own CI uses it. |
| lychee | Link checker for local files and URLs; the Kit's own CI uses it. |

## Usage pacing

Role: keep shared budgets, such as API calls and model usage, from running out early.
Watch shared API budgets before bulk loops, and stop below a safe remainder.
