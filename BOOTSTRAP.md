# Bootstrap

This playbook fits the Kit to one Project.
An agent follows it from start to end.
It works for a new, empty Project and for an existing Project with code, rules, and history.

## Rules for the agent that bootstraps

- Talk to the Owner in plain language.
  Do not assume that the Owner can read code.
- Ask one question per message.
  Give your recommendation and the reason in one or two sentences.
- Find facts yourself.
  Ask the Owner only for Decisions.
- Never overwrite an existing file, rule, hook, or CI job without the Owner's answer.
- The Kit names no tech stack.
  Take every stack fact from the Project itself, in the order that [core/agents/engineering.md](./core/agents/engineering.md#mandatory-external-facts) gives.
- When the Project's stack convention contradicts a Kit rule, stop and ask the Owner.
  Show both sides and your recommendation.
  Record the answer as an ADR so that Upgrade does not ask again.
- Make no Decision for the Owner.
  Choose defaults only for things this playbook marks as defaults.

## Step 1: read the Kit

Read these Kit files whole, in this order:

1. [GLOSSARY.md](./GLOSSARY.md)
2. [core/rules-file.md](./core/rules-file.md) and [core/owner-guide.md](./core/owner-guide.md)
3. Every page in [core/agents/](./core/agents/)
4. [checks/README.md](./checks/README.md)
5. [skills/README.md](./skills/README.md)
6. [adapters/agents.md](./adapters/agents.md) and [adapters/trackers.md](./adapters/trackers.md)

Record the Kit version: the newest release tag, or the commit id when there is no tag.

Done when you can say which Project files Bootstrap writes and which checks it builds.

## Step 2: survey the Project

The Survey is read-only.
It writes nothing and runs nothing that changes state.

Find and write down:

- **Shape**: empty, or existing code.
- **Stack**: languages, runtimes, frameworks, package managers, databases, and their installed versions, from manifests, lockfiles, image tags, and `--version` output.
- **Stack conventions**: where the stack expects tests, config, migrations, and generated files to live; how it formats and lints; what it treats as the entry point.
  Read the docs that ship with the installed versions first.
- **Existing agent rules**: every rules file, skill folder, and agent config that [adapters/agents.md](./adapters/agents.md) lists.
- **Existing checks**: hooks, lint and format config, type checking, test commands, CI jobs, branch protection.
- **Documents**: README, glossaries, ADRs, design notes, runbooks.
- **Tracker**: where issues live, and the labels that exist.
- **History**: reverts, repeated fix commits on the same files, flaky tests, and incidents.
  These are candidate Corrections.
- **Risk areas**: code that touches authentication, authorization, tenant or user isolation, money, personal data, secrets, schema migrations, destructive jobs, deployment, and production access.
  These seed the tier map.
- **Runtime**: how the application starts locally, what it needs (database, queue, credentials), and whether two copies can run at once.

Done when every item above has an answer or the word UNKNOWN.

## Step 3: interview the Owner

Ask these in order, one per message.
Skip a question when the Survey already answers it beyond doubt, and tell the Owner the answer you found.

1. **Purpose.**
   "In two or three sentences, what does this project do, and who uses it?"
   Use the answer to seed the Project glossary.
2. **Review mode.**
   "Do you want to review code? Always, only for risky changes, or never?"
   Explain the three modes from [core/agents/review-and-merge.md](./core/agents/review-and-merge.md#mandatory-review-modes) in plain words.
   Tell the Owner that they make every Decision in every mode, and that they can change the mode at any time.
3. **Stack, for an empty Project only.**
   The stack is a Decision.
   Recommend one in plain words: what it is good at, what it costs to run, and how common it is.
   Verify every version you name before you recommend it.
4. **Agents.**
   "Which coding agents will work here?"
   Default: every agent in [adapters/agents.md](./adapters/agents.md).
5. **Tracker.**
   Default: the Project's existing tracker, else the one [adapters/trackers.md](./adapters/trackers.md) names as default.
6. **Branches.**
   Default: one long-lived branch, one short-lived branch and one pull request per task.
   Ask only when the Project deploys from more than one branch.
7. **Risk areas.**
   Show the tier map draft from the Survey in plain words, for example "changes to sign-in and payments need your approval".
   Ask the Owner to add or remove areas.
8. **Forbidden actions.**
   "Is there anything an agent must never do here?"
   Start from the list in [core/agents/security.md](./core/agents/security.md#mandatory-forbidden-actions) and ask what to add.
9. **Stack-convention conflicts.**
   One message per conflict that the Survey found.
10. **Adoption plan conflicts, for an existing Project only.**
    One message per conflict in Step 4.

Done when every question has an answer.

## Step 4: write the plan

Write the plan as one page and show it to the Owner.
For an existing Project, this is the Adoption plan.

The plan lists:

- Each file that Bootstrap will create, with one line about its purpose.
- Each existing file that Bootstrap will move or merge, and where its content goes.
  Existing rules move into the Project rules file; they are not dropped.
- Each check that Bootstrap will build, and each check spec that an existing check already covers.
- Each conflict and the Owner's answer.
- Each label that Bootstrap will create in the tracker.
- For an existing Project, one `cleanup` issue per module that holds comments, dead code, or duplicate code.

Get the Owner's approval of the plan before Step 5.

## Step 5: write the Project files

Work on a new branch.
Write these, fitted to the Project:

| Project path                               | Source in the Kit                                                    |
| ------------------------------------------ | -------------------------------------------------------------------- |
| Rules file, with agent file names as links | [core/rules-file.md](./core/rules-file.md), [adapters/agents.md](./adapters/agents.md) |
| `docs/owner-guide.md`                      | [core/owner-guide.md](./core/owner-guide.md), with the agents, Review mode, and Project skills filled in |
| `docs/agents/*.md`                         | [core/agents/](./core/agents/), with Project commands and paths filled in |
| `docs/checks/*.md`                         | The check specs that the Project builds, each with its "Implemented by" line |
| `docs/adr/`                                | One ADR for the Review mode; one per stack-convention conflict; one per rejected Kit rule |
| `GLOSSARY.md`                              | [templates/glossary.md](./templates/glossary.md), seeded from the Purpose answer |
| Skills folder and `skills-lock.json`       | [skills/README.md](./skills/README.md): copy the Kit's own skills, install every pinned skill |
| Tracker labels                             | [core/agents/issue-tracker.md](./core/agents/issue-tracker.md#mandatory-labels) |

Rules for this step:

- Replace every placeholder in angle brackets with a Project fact.
  A placeholder left in a file is a failed Bootstrap.
- Fill in commands only after you run them once and see them work.
  Write UNKNOWN and tell the Owner when a command does not exist yet.
- Put the Kit version and the Bootstrap date in the rules file, where [core/rules-file.md](./core/rules-file.md) shows.
- Do not create the Verification contract now.
  [core/agents/verification.md](./core/agents/verification.md) creates it with the first user-visible feature.
- In an existing Project, keep every existing rule that the Owner kept.
  Put it where the Kit layout says it belongs.
- In an existing Project with `CONTEXT.md` glossaries, move each one to `GLOSSARY.md` with a Git move, and update every link to it.
- Install the pinned skills with the commands in [adapters/tools.md](./adapters/tools.md#skills-installer).
  Compare each hash in `skills-lock.json` with [skills/README.md](./skills/README.md).
  A different hash means the upstream skill changed: read the change, and ask the Owner before you keep it.
- Fill the JSON compactor command in `.rules` from [adapters/tools.md](./adapters/tools.md#json-compactor), or from the Owner's own tool for that role.
  Run it once on a JSON sample before you write it in.
- Fill the Agent skills block in `.rules` and the operations section of `docs/agents/issue-tracker.md`, because the pinned skills read them to find the tracker.

## Step 6: build the checks

For each check spec in the plan:

1. Read the check spec.
2. Build the check in the Project's own stack, where the spec says it runs: a hook, CI, or both.
3. Run the check on the spec's failing case.
   It must fail, with the spec's error text or a clear equivalent.
4. Run the check on the spec's passing case.
   It must pass.
5. Write the Project's `docs/checks/<name>.md`: the spec's invariant, then "Implemented by", the command that runs it, and where it runs.

A check that you cannot prove both ways is not done.
After two failed attempts on one check, record it in the plan as open, tell the Owner, and continue with the next check.

When you build [feedback-budget](./checks/feedback-budget.md), measure each Feedback loop once.
A loop that is already over its budget is a conflict for the Owner.
Recommend making the loop fast before the check turns on.
When the Owner chooses a higher budget for now, record it in an ADR with the date to return to the Default, and open a `budget` issue.

## Step 7: prove the setup

1. Start a fresh agent session that has seen none of this work.
   Give it only the Project.
2. Ask it these questions, and compare each answer with the files:
   - "Where do tests for a new feature go, and which command runs them?"
   - "Which tier is a change to `<a risk-area path>`, and who must approve it?"
   - "What must you do before your first edit?"
   - "What do you do after two failed attempts to fix one test?"
   - "Where does a new domain term go?"
   - "The Owner approved an idea. How does it become work, and which Ticket do you start first?"
   - "The Owner has a new feature idea. What exactly does the Owner type first?"
3. Fix each file that led to a wrong answer, and ask again.
4. Run every check once on the whole Project.
   Record any existing failure as known red; do not fix it in the Bootstrap change.

## Step 8: hand over

1. Commit with the Project's commit rules.
2. Open one pull request.
   The Bootstrap change touches CI and policy, so it is tier 3.
   It needs the Owner's approval in every Review mode.
3. Give the Owner a Risk summary from [templates/risk-summary.md](./templates/risk-summary.md):
   - what the Project now has, in plain words;
   - which checks run, and where;
   - the open items and the known-red list;
   - what to do next: usually the first real task, with the exact message to type from `docs/owner-guide.md`.
4. Show the Owner `docs/owner-guide.md`, and offer to walk through its first step on that first task.
5. After the merge, create the tracker labels, then the `cleanup` issues from the plan.

Bootstrap is done when the pull request is merged and the labels exist.
