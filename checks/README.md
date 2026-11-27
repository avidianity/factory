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
- **Passing case**: an input that the check must accept, including a false-positive trap when the check parses code or diffs.
- **Error text**: what the check prints, which names the rule and the fix.

## Building a check

1. Read the check spec.
2. Find the tool in the Project's stack that fits: a compiler, a linter, a syntax-tree parser, a test, a hook script, or a CI step.
   Prefer a tool that the Project already has, or a recommended default tool from [adapters/tools.md](../adapters/tools.md).
3. For checks that parse source code (`no-comments`, `no-dead-code`, `no-duplicate-code`, `no-suppressions`, `sql-parameterization`, and reverse-dependency lookup in `risk-tier`), use a compiler, linter, or syntax-tree parser.
   Never build a syntax-aware check as a custom regular-expression script over raw source text.
   When no suitable tool exists in the Project's stack for an Earned check, leave the check open in the Adoption plan instead of shipping a fragile script.
4. Build the check where the spec says it runs.
5. Run it on every failing case, and confirm that it fails with the error text or a clear equivalent.
6. Run it on every passing case, including false-positive traps, and confirm that it passes.
   For a configured tool (such as a linter, formatter, commit-message checker, or secret scanner), run the tool on the sample inputs and record the result; never commit a unit test file for its configuration file.
   For a custom check script whose policy logic merits automated self-tests, put those tests in the separate check-test path that [test-scope](./test-scope.md) defines, never in the application test suite.
7. Write `docs/checks/<name>.md` in the Project: the invariant, then "Implemented by", the command, and where it runs.

A check that you did not prove both ways is not done.
A check that a hook runs must also run in CI, because a hook can be skipped on a machine that lacks it.

## Day-1 core specs

Bootstrap builds every Day-1 core spec whose condition holds for the Project.
When a batteries-included framework or library supersedes contradicting Kit conventions in a stack ADR, omit any check spec that contradicts that framework's native workflow.

| Spec | Applies when |
| ---- | ------------ |
| [trusted-ci](./trusted-ci.md) | Always |
| [risk-tier](./risk-tier.md) | Always |
| [test-oracle-guard](./test-oracle-guard.md) | Always |
| [no-hook-bypass](./no-hook-bypass.md) | Always |
| [secret-scan](./secret-scan.md) | Always |
| [merged-result](./merged-result.md) | Always |
| [evidence-bundle](./evidence-bundle.md) | Always |
| [agent-entry-links](./agent-entry-links.md) | Always |
| [skills-link](./skills-link.md) | The Project has skills |
| [skills-in-owner-guide](./skills-in-owner-guide.md) | The Project has skills |
| [tool-servers-render](./tool-servers-render.md) | The Project configures agent tool servers |
| [doc-size](./doc-size.md) | Always |
| [doc-index](./doc-index.md) | Always |
| [doc-links](./doc-links.md) | Always |
| [no-placeholders](./no-placeholders.md) | Always |
| [doc-style](./doc-style.md) | Always |
| [record-numbers](./record-numbers.md) | Always |
| [commit-message](./commit-message.md) | Always |
| [dependency-pinning](./dependency-pinning.md) | The Project has dependencies |
| [behavior-has-test](./behavior-has-test.md) | The Project has application code |
| [test-scope](./test-scope.md) | The Project has application code |
| [no-suppressions](./no-suppressions.md) | The Project has a linter or type checker |
| [migration-history](./migration-history.md) | The Project has SQL migrations not superseded by a batteries-included framework's native migration workflow |
| [migration-apply](./migration-apply.md) | The Project has SQL migrations not superseded by a batteries-included framework's native migration workflow |

## Earned specs

Earned specs prevent real problems once a codebase grows, but need real stack tools or baseline evidence to earn their maintenance cost; see [methods/trust-layers.md](../methods/trust-layers.md#judging-a-new-check).
Bootstrap builds an Earned spec only when a suitable tool already exists in the Project's stack and the Owner opts in.
Otherwise, build it when a Correction or codebase growth proves the need.

| Spec | Applies when |
| ---- | ------------ |
| [no-comments](./no-comments.md) | The Project has application code and a syntax-tree linter or parser |
| [no-dead-code](./no-dead-code.md) | The Project has application code and a reachability or dead-code tool |
| [no-duplicate-code](./no-duplicate-code.md) | The Project has application code and a clone detector |
| [feedback-budget](./feedback-budget.md) | The Project has automated checks or tests with measurable CI runtime |
| [config-access](./config-access.md) | The Project has application code |
| [no-network-in-tests](./no-network-in-tests.md) | The Project has automated tests |
| [doc-freshness](./doc-freshness.md) | The Project has application code and domain docs (`HOW-TO.md`, `STATE.md`, or `MAP.md`) |
| [sql-parameterization](./sql-parameterization.md) | The Project writes raw SQL in application code, has a syntax-tree linter or type checker, and is not superseded by a framework query layer |

## Adding a check spec to a Project

A Correction often ends in a new check.
Write its spec in the Project's `docs/checks/` with the same format.
When the check is useful beyond one Project, propose it to the Kit.
