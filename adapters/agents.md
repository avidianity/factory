# Coding agent adapter

This page tells Bootstrap where each coding agent reads each kind of setup.
The goal is one source per kind, so that every agent gets the same rules, skills, and tools.

Last checked: 2026-11-14, against Claude Code 2.1.325, Codex 0.169.0, and OpenCode 2.2.1.

## Where each agent reads

| Kind | Claude Code | Codex | OpenCode |
| ---- | ----------- | ----- | -------- |
| Rules | `CLAUDE.md`; reads `AGENTS.md` only when no `CLAUDE.md` exists | `AGENTS.md`, at most 32 KiB of project instructions in total | `AGENTS.md` |
| Project skills | `.claude/skills` | `.agents/skills` | `.agents/skills`, `.claude/skills`, `.opencode/skills` |
| Project tool servers (MCP) | `.mcp.json` | `.codex/config.toml`, in a trusted project | `opencode.json` |
| Tool-server header helper | `headersHelper` | `http_headers_helper`, run with a cleared environment | none |
| Subagent definitions | `.claude/agents` | `.codex/agents` | `.opencode/agents` |
| General subagent | Agent tool, `subagent_type` `general-purpose` | `spawn_agent` | `Task` tool, `subagent_type` `general` |

Sources: each agent's official docs on memory or instructions, skills, MCP, and subagents.

## How the Owner starts a skill

Last checked: 2026-11-21, against Claude Code 2.1.329, Codex 0.169.0, and OpenCode 2.2.1.

| Kind | Claude Code | Codex | OpenCode |
| ---- | ----------- | ----- | -------- |
| Start a skill | Type `/<skill>`; the request follows on the same line, as in `/implement <Ticket link>` | Type `$<skill>` anywhere in the message; the rest of the message goes with it | Type `@<skill>` in the message; the rest of the message goes with it |
| List skills | `/skills`, or type `/` | `/skills`, or type `$` | Type `@` |
| Honors `disable-model-invocation: true` | Yes: the model cannot start the skill, and the Owner can | No: Codex reads only `name` and `description`; the model can still start the skill | Yes, as an alias of `metadata.opencode/autoinvoke: false` |

In Codex, `policy.allow_implicit_invocation: false` in a skill's `agents/openai.yaml` hides it from the model.
Do not add that file to a pinned skill; a pinned skill stays as the lock file records it.
The rule in [core/agents/planning.md](../core/agents/planning.md#mandatory-how-agents-run-pinned-skills) still forbids an agent to start such a skill on its own.

Sources: <https://code.claude.com/docs/en/skills.md>, <https://developers.openai.com/codex/skills.md> and the Codex source at tag `rust-v0.169.0`, and <https://opencode.ai/docs/skills.md>.

## The one-source layout

| Kind | Source | Links or generated files |
| ---- | ------ | ------------------------ |
| Rules | `.rules` | `AGENTS.md` and `CLAUDE.md` are symbolic links to `.rules` |
| Skills | `.agents/skills` | `.claude/skills` is a symbolic link to `.agents/skills` |
| Tool servers | One source file in the Project's own language | `.mcp.json`, `.codex/config.toml`, and `opencode.json`, generated, never edited by hand |
| Subagents | Not committed | Each developer brings their own; [core/agents/delegation.md](../core/agents/delegation.md) describes the roles |

Bootstrap creates only the links and files for the agents the Owner named.
The check specs [agent-entry-links](../checks/agent-entry-links.md), [skills-link](../checks/skills-link.md), and [tool-servers-render](../checks/tool-servers-render.md) keep the layout correct.

## Rules for agent-specific content

- Do not add a skill that works in one agent only, such as a skill that installs one agent's hooks.
- When a skill calls an agent's own tool, name the tool for each agent in the skill.
- Codex loads `.codex/config.toml` only after its user marks the checkout as trusted in their own Codex config.
- Keep `.rules` under 32 KiB, because Codex cuts project instructions off at that size.

## Command guards

A command guard refuses a shell command before the agent runs it.
Use one for the [no-hook-bypass](../checks/no-hook-bypass.md) check where the agent supports it:

- Claude Code: a `PreToolUse` hook on the shell tool.
- Codex: check the installed version's hooks docs.
- OpenCode: check the installed version's plugin docs.

A guard is a second line.
CI parity is the first line, because a guard does not run on every machine.
