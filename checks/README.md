# Check specs

A check spec states one rule that a machine can decide.
It names no tech stack.
A Project builds each check spec as a real check in its own stack, and proves it both ways before Bootstrap is done.

## Format

Every check spec has these sections:

- **Invariant**: the rule, in one or two sentences.
- **Why**: the failure it prevents.
- **Applies when**: always, or the condition under which the Project needs it.
- **Runs in**: hook, CI, or both.
- **Failing case**: an input that the check must reject.
- **Passing case**: an input that the check must accept.
- **Error text**: what the check prints, which names the rule and the fix.

## Building a check

1. Read the check spec.
2. Find the tool in the Project's stack that fits: a lint rule, a test, a hook script, or a CI step.
   Prefer a tool that the Project already has.
3. Build the check where the spec says it runs.
4. Run it on the failing case, and confirm that it fails with the error text or a clear equivalent.
5. Run it on the passing case, and confirm that it passes.
6. Write `docs/checks/<name>.md` in the Project: the invariant, then "Implemented by", the command, and where it runs.

A check that you did not prove both ways is not done.
A check that a hook runs must also run in CI, because a hook can be skipped on a machine that lacks it.

## The specs

| Spec | Applies when |
| ---- | ------------ |
| [agent-entry-links](./agent-entry-links.md) | Always |
| [skills-link](./skills-link.md) | The Project has skills |
| [tool-servers-render](./tool-servers-render.md) | The Project configures agent tool servers |
| [doc-size](./doc-size.md) | Always |
| [doc-links](./doc-links.md) | Always |
| [no-placeholders](./no-placeholders.md) | Always |
| [doc-style](./doc-style.md) | Always |
| [record-numbers](./record-numbers.md) | Always |
| [risk-tier](./risk-tier.md) | Always |
| [test-oracle-guard](./test-oracle-guard.md) | Always |
| [evidence-bundle](./evidence-bundle.md) | Always |
| [behavior-has-test](./behavior-has-test.md) | The Project has application code |
| [commit-message](./commit-message.md) | Always |
| [no-hook-bypass](./no-hook-bypass.md) | Always |
| [secret-scan](./secret-scan.md) | Always |
| [dependency-pinning](./dependency-pinning.md) | The Project has dependencies |
| [config-access](./config-access.md) | The Project has application code |
| [no-suppressions](./no-suppressions.md) | The Project has a linter or type checker |
| [no-network-in-tests](./no-network-in-tests.md) | The Project has automated tests |
| [merged-result](./merged-result.md) | Always |
| [feedback-budget](./feedback-budget.md) | Always |
| [no-comments](./no-comments.md) | The Project has application code |
| [no-dead-code](./no-dead-code.md) | The Project has application code |
| [no-duplicate-code](./no-duplicate-code.md) | The Project has application code |

## Adding a check spec to a Project

A Correction often ends in a new check.
Write its spec in the Project's `docs/checks/` with the same format.
When the check is useful beyond one Project, propose it to the Kit.
