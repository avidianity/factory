# Testing by defect class

Choose a test by the kind of defect it must catch, not by fashion.

## First tool by defect class

| Defect class | First tool | Negative or adversarial test | Escalate to |
| ------------ | ---------- | ---------------------------- | ----------- |
| Wrong type or missed branch | The type system, with exhaustive unions | Leave out a variant; pass the wrong kind of value | Runtime validation when outside data can break the type |
| Invalid request payload | Request schemas and contract tests | Send malformed, extra, missing, and out-of-range fields | Property-based fuzzing of parsers |
| Coupling between modules | Dependency rules | Import a forbidden internal module | Redesign the API when exceptions multiply |
| Data leak between tenants | Integration tests on the real database and authorization layer | Use another tenant's identifier, joins, and exports | Property or model tests for multi-step flows |
| Double charge | A transaction, a unique key, and idempotent handler tests | Send duplicate requests at the same time | Model checking when the interleavings stay unclear |
| Duplicate side effect on retry | Worker integration with a fake provider | Crash between the side effect and the acknowledgement | Fault injection or model checking |
| Wrong result in the user interface | Runtime verification with a browser driver | Wrong role, failed request, stale cache | Broader generated journeys |
| Hidden branch bug | Targeted mutation testing | Flip a comparison, a negation, a default, or remove a validation | Stronger behavior assertions |
| Unknown input edge | Property-based testing | Generated text, sizes, orders, dates, and numbers | A formal property for a costly invariant |
| Interface state explosion | Exploration guided by Feature maps | Navigation permutations and invalid transitions | A model-based interface harness |
| Performance | Profiling and benchmark replay | Cold and warm runs, load, release build, data volume | A long-running performance suite |
| Prompt injection | A test with untrusted content | A hostile file or tool result tells the agent to leak data | Tool restrictions at the policy level |
| Configuration or deployment | Canary release, smoke tests, rollback rehearsal | A missing setting, a stale migration, a failed provider | Release verification and alerts |
| SQL or schema contract drift | Schema-derived types, database-backed SQL checks, or real-engine integration tests | Pass nulls, boundary numbers, decimals, dates, and JSON through the real driver | Freshness check in CI plus real-engine contract suite |
| Unsafe dynamic SQL construction | Parameterized queries, explicit allow-lists, and syntax-tree lint or type rules | Pass hostile input in values, sort keys, and filter names | Strict typed query wrappers or driver identifier helpers |
| Unsafe schema or data migration | Clean-create and baseline upgrade verification on a disposable target engine | Run migrations on synthetic rows with constraints, concurrent writes, and interrupted backfills | Expand, migrate, switch, contract rollout and tested restore drill |
| Invalid tool or build config | The tool itself, its schema, and the type checker | Run the tool on one failing sample when changing the config | CI execution of the tool, never a config unit test |

## When no automated test is the right tool

Not every file in a repository earns an automated test.
Adding tests for files that a lower layer already checks slows down every Feedback loop and adds maintenance drag without catching product bugs.

- **Declarative tool and build configs** (linter, formatter, commit-message, compiler, bundler, database-tool, and container configs): run the tool or type checker; never commit unit tests that mirror config values.
- **Low-risk developer and repository scripts** (such as local setup helpers or formatting wrappers): verify by running the script when it changes.
  Base exceptions on risk, never on change frequency: deployment, backup, data-migration, and custom policy check scripts earn targeted tests or rehearsals in a separate non-application test path triggered when those scripts, their configuration, or lockfile dependencies change; see [test-scope](../checks/test-scope.md).
- **Type-only files, constants, generated files, and thin wiring**: let the type system, compiler, and real boundary tests cover them.

Apply the same boundary to coverage, complexity-coverage, and mutation tools: collect only an explicit allow-list of application behavior paths and exclude configs, scripts, generated files, migrations, type-only files, thin entry points, and test helpers.
Never add filler tests to raise coverage on excluded files; fix the tool's scope instead.

## Mutation testing

Mutation testing asks: when we break the code on purpose, does a test notice?
It measures how well assertions discriminate.
It complements, and does not replace, a map of states and transitions.

Useful mutants:

- Remove or invert the tenant condition in a query.
- Change `>=` to `>` on a balance or limit check.
- Delete a uniqueness check on an idempotency key.
- Map a failed payment to a success state.
- Remove the refusal of a forbidden state transition.
- Weaken an `AND` filter to `OR`.
- Drop an authorization check on one route.
- Turn a required row limit into no limit.

A mutant counts as killed only when a test fails for the intended business rule.
A failure from brittle naming or broken setup is not a kill.

Read results by kind:

- **Killed**: a test detected the change.
- **Survived**: the tests passed; check whether this is a gap.
- **Equivalent**: the change cannot alter behavior; it cannot be killed.
- **Timeout or error**: may be harness noise.
- **Uncovered**: no test runs that path.

A score of 100 percent is not the goal.
Scope mutation testing strictly to high-risk application domain code, excluding config files, scripts, and glue code.
Kill the high-risk survivors first.
One killed authorization mutant is worth more than five points of score in glue code.

## Property-based tests and metamorphic relations

When an exact expected output is hard to state, a metamorphic relation can still express correctness:

- Adding a row for tenant B does not change tenant A's totals.
- Reordering independent rows does not change a deterministic aggregate.
- Applying the same idempotency key twice does not double the effect.
- Encoding then decoding a supported value returns the same value.
- Aggregating partitions then combining them equals aggregating the whole.
- Running a read twice does not change stored data or charge twice.
- A refused request emits no external side effect.

Check each property with hand-written passing and failing examples first, because a generated oracle can be wrong.
Fix random seeds, keep each minimized counterexample as a fixed test, and replay it after the fix.

## Interface verification beyond a screenshot

A golden path can pass while loading, empty, error, unauthorized, retry, and large-data states are broken.
For each feature, keep the evidence that [core/agents/verification.md](../core/agents/verification.md#mandatory-evidence-for-a-user-visible-change) lists.

## Checks that are hard to game

Agents can make a check pass without satisfying it.
Watch for:

- rewriting a test to assert the produced output instead of the specified output;
- hardcoding a result for a fixture;
- mocking a security check that runs in production;
- testing an old build or the wrong checkout;
- skipping or renaming a failing test;
- replacing a hard assertion with a screenshot or a story;
- adding a suppression directive;
- moving code to a path the linter does not scan;
- raising a timeout to hide a hang;
- muting error logs so that alerts stay quiet;
- passing on agent agreement or a generated review summary alone.

Defenses: protected acceptance fixtures, protected CI definitions, explicit approval to change a test oracle, evidence tied to the exact revision, and an Independent reviewer for Tier 2 and above.
The check spec [test-oracle-guard](../checks/test-oracle-guard.md) raises the Tier of changes that weaken checks.
