# Factory

Factory is a kit of documents that sets up a software project for development with AI coding agents.
It gives a new or existing project its rules, checks, review policy, and agent workflows before real development starts.
It holds no code and names no tech stack.
The agent that bootstraps a project reads the project's own stack and fits the kit to it.

- Author: John Michael Manlupig
- Email: <manlupigjohnmichael@gmail.com>
- License: [MIT](./LICENSE)

## What a project gets

- One rules file that every coding agent reads, with a short reading map to task pages.
- Task pages for engineering, testing, git, review and merge, delegation, documentation, domain language, the issue tracker, runtime verification, corrections, and security.
- A set of check specs, each built as a real check in the project's own stack and proved with a failing case and a passing case.
- A risk tier for every change, set by a check from the changed paths, never by the agent.
- A review mode that fits the person who owns the project, from "I review every diff" to "I do not read code".
- Light records that show what to enforce next and when autonomy is earned: corrections, evidence in each pull request, and escaped defects.

## Start

Give your coding agent this instruction in the project folder:

```text
Bootstrap this project from https://github.com/avidianity/factory.
Read BOOTSTRAP.md there and follow it.
```

The agent works for new and existing projects.
In an existing project it never overwrites; it writes an adoption plan and asks you about each conflict.

To bring a bootstrapped project up to a newer kit version:

```text
Upgrade this project from https://github.com/avidianity/factory.
Read UPGRADE.md there and follow it.
```

## How the kit is organized

| Path                         | Holds                                                                                    |
| ---------------------------- | ---------------------------------------------------------------------------------------- |
| [BOOTSTRAP.md](./BOOTSTRAP.md) | The playbook an agent follows to set up a project                                      |
| [UPGRADE.md](./UPGRADE.md)   | The playbook an agent follows to apply newer kit changes to a project                    |
| [CONTEXT.md](./CONTEXT.md)   | The kit's glossary                                                                       |
| [core/](./core/)             | The rules file template and the task pages a project receives                            |
| [checks/](./checks/)         | Check specs: the invariant, a failing case, a passing case, where it runs, its error text |
| [templates/](./templates/)   | Task contract, correction, ADR, glossary, HOW-TO, feature map, evidence, risk summary     |
| [methods/](./methods/)       | Reference methods: trust layers, tests by defect class, formal methods, review prompts   |
| [skills/](./skills/)         | The kit's own skills and the manifest of third-party skills a project installs           |
| [adapters/](./adapters/)     | The only pages that name agents, trackers, or tools                                      |
| [fleet/](./fleet/)           | The operating model for many agent sessions at once; never copied into a project         |

## Principles

The kit rests on these ideas.
[methods/trust-layers.md](./methods/trust-layers.md) explains them in full.

1. Trust comes from the environment, not from instructions.
   A good API, a database constraint, a type, a lint rule, a test, and a runtime check each beat a paragraph of prose.
2. Each repeated agent mistake becomes a check at the lowest layer that can stop it.
   A rule that only lives in prose is a rule an agent can ignore.
3. Prove one verified loop before you add more agents or more autonomy.
4. The human makes every decision.
   Code review is a separate thing, and the project chooses who does it per risk tier.
5. Each fact has one home.
   Every other document links to it.

## Writing rules for the kit

- Write in Simplified Technical English: short sentences, active voice, one meaning per word.
- Put each sentence on its own line.
- Do not use the em dash.
- Use the terms in [CONTEXT.md](./CONTEXT.md) as written.
- Name no tech stack anywhere.
  Name agents, trackers, and tools only in [adapters/](./adapters/) and in the kit's own CI.
- Keep each page under 32,000 bytes, because agents read some pages whole.

## Credits

The skills manifest installs five skills by Matt Pocock from [mattpocock/skills](https://github.com/mattpocock/skills).
The kit does not copy them.
