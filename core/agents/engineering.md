# Engineering instructions

This page applies when you write or review application code, configuration, or dependencies.

## Mandatory: external facts

Treat your training data as stale for every version-specific fact outside this repository.
That covers library and framework APIs, CLI flags, configuration keys and schemas, container and vendor service behavior, model IDs, and prices.
The repository's own code and the language syntax need no external check, because the code is its own source.

Verify each such fact before you write code on it or recommend it.
One check per fact per session is enough.
Look it up in this order:

1. Find the installed version from the lockfile, the image tag, or `<tool> --version`.
2. Read the docs that ship inside the installed package or tool, such as a bundled docs folder, an `llms.txt` file, or `<tool> --help`.
3. Read the official docs for that version on the web.
   Prefer a Markdown or `llms.txt` endpoint.
4. Read the installed type definitions or source to settle what steps 2 and 3 leave open.
5. Treat blogs, forums, and answer sites as leads only.
   Confirm each lead against steps 2 to 4.

When a decision rests on such a fact, name the source in one line of your reply, for example: `Option X exists since 6.0; installed 8.1 (vendor docs page).`
Keep these citations in the reply, out of code comments and commit messages.

When a repository document or a memory contradicts the current docs, say so and correct the stale claim in the same change.
Correct only the factual claim; the decision built on it may still stand for other reasons.

## Mandatory: stack-convention conflicts

The Kit names no stack, so a Kit rule can contradict what the Project's language, framework, or tool expects.
Examples: the stack wants tests in a separate tree, but a Kit default says colocate them; the stack's generator writes files that a Kit check rejects.

When you find such a conflict:

1. Stop before you choose a side.
2. Ask the Owner in plain words: what the stack expects, what the Kit rule says, and your recommendation with one reason.
3. Record the answer in an ADR that names both rules.
4. Follow the ADR from then on.

An existing ADR settles the conflict; do not ask again.

## Default: how to weigh choices

Prefer quality, simplicity, robustness, scalability, and long-term maintainability.
Give development cost little weight.
A slower path that removes a class of defects beats a faster path that needs a rule to stay safe.

## Mandatory: dependencies

- Use one library per concern.
  Before you add a library, check whether the Project already has one for that concern.
- Pin exact versions, and commit the lockfile.
- Review the dependency diff in the lockfile before you commit a dependency change.
- A dependency change is at least Tier 2; see [review and merge](./review-and-merge.md#mandatory-tiers).
- Read the installed version's docs before you use a new API, as the external-facts order says.

## Mandatory: configuration and secrets

- Read environment variables and other runtime configuration only in the Project's configuration modules.
  Every other module receives configuration as a typed value.
- Validate configuration at startup and fail fast on a missing or invalid value.
- A secret has no default value.
- Put timeouts, limits, and sizes in configuration with safe defaults, not as literals spread through the code.
- Never commit a secret, a token, or production data.

## Mandatory: errors and diagnostics

- A catch block binds the error and handles it or passes it on.
  Never swallow an error silently.
- A wrapped error keeps the original as its cause.
- A diagnostic rule is either an error or off.
  Do not leave warnings that nobody reads.
- Do not add a suppression directive to silence a lint or type error.
  Fix the cause, or change the rule for the whole Project through an ADR.

## Default: design

- Make invalid states impossible to represent before you add a check that detects them.
  [Corrections](./corrections.md#mandatory-decision-order) gives the order of layers.
- Name modules and functions by the ability they give to callers, not by the method they use inside.
- Prefer the stack's conventions and simple boundaries.
  Do not add layers of interfaces only to look clean.
- Make each check's error text name the violated rule and the approved alternative.
