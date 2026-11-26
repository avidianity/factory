# The Owner guide

Every Project has one Owner guide at `docs/owner-guide.md`.
It is the tutorial for the Owner: which skill to start in each situation, what to type, what comes back, and what the Owner approves.
Agents follow `.rules` and `docs/agents/`; the Owner reads this page instead.
This page gives its template and the rules for keeping it correct.

## Why it exists

The pipeline in [agents/planning.md](./agents/planning.md) tells agents when each skill runs.
Most pinned skills start only when the Owner asks for them.
An Owner who does not know a skill's name, or what it expects, cannot start it.
The Owner guide closes that gap, so that the Owner never has to read a `SKILL.md` to know what to type.

## Template

Copy the block below into the Project's `docs/owner-guide.md`.
Replace every placeholder in angle brackets.

````markdown
# Working with agents in <Project name>

This page is for you, the Owner.
It tells you which skill to start, what to type, what you get back, and what you approve.
Agents follow `.rules` and `docs/agents/`; you do not need to read them.

## Your job and the agents' job

- You make every Decision: a choice about product behavior, data, cost, security, or architecture.
  An agent asks; it never answers its own questions.
  You may pre-approve bounded routine choices in one Standing Decisions ADR; Tier 3 and Tier 4 choices are never delegated.
- Agents find facts, write the code, run the checks, and prove that the change works.
- The Review mode is <Always | Critical only | Never>.
  <One sentence from the Review mode table for this mode: what the Owner reads before a merge.>
  You can change it at any time; ask an agent to record the new mode in an ADR.

## How to start a skill

A skill is a written procedure that an agent follows.
Most skills here start only when you ask for one, so an agent never starts them alone.
Starting a skill is your permission for the work it describes, and for nothing more.

| Agent | Start a skill | See all skills |
| ----- | ------------- | -------------- |
| <agent> | <how the Owner starts a skill in this agent> | <how the Owner lists skills in this agent> |

Put your request after the skill name, on the same line:

```text
<skill start form for grill-with-docs> Members want a reminder the day before each booking.
```

When a command does not work, ask in plain words, for example "Run the grill-with-docs skill."
The rules tell the agent to read the skill and follow it.

## Pick a skill

| When you | Start | You get |
| -------- | ----- | ------- |
| Have an idea for a feature or a change | `grill-with-docs` | Questions until every Decision is settled; ADRs and glossary terms |
| Have an idea too big for one session, or too foggy to describe | `wayfinder` | A Map issue that lists the Decisions to make, one per session |
| Got a bug report or a feature request | `triage` | A labelled issue; `ready-for-agent` when an agent can do it alone |
| Settled the Decisions in this session | `to-spec` | One Spec issue |
| Have a Spec | `to-tickets` | A Ticket breakdown to approve, then Tickets in the tracker |
| Want one Ticket built | `implement` | Commits on a branch and a pull request |
| Want a whole Spec built | `implement-spec` | Every Ticket built on one branch, and one pull request |
| Want a design for a change before any code | `architect` | A Design sketch: types and signatures with no bodies |
| See two designs that stay close | `arena` | Several agents try the design; you get the best one, with parts of the others |
| Want to know what a change can break | `blast-radius` | What breaks outside the change, and the Safety fact, proven by a run |
| Want a change attacked before it merges | `interrogate` | Only the problems that a run proved; it fixes nothing |
| Want a diff reviewed against the Spec and the rules | `code-review` | Findings on two axes: the Project's rules, and the Spec |
| Think a task description is too thin | `sharpen` | A deeper version of the same task, with the same scope |
| Start a long run that you will not watch | `show-me-your-work` | A decision log: one line per choice, with its reason and proof |
| Want your plan questioned, with no documents written | `grill-me` | Questions until the plan holds up |
| Did not understand an agent's last message | `wait-what` | The same message again, in plain words |
| Saw the same agent mistake a second time | `correction` | A `correction` issue, and later a check that stops the mistake |
<One row for each Project skill: when the Owner starts it, its name, and what the Owner gets.>

A small, clear change needs no skill.
Ask for it in plain words, for example "Rename the Export button to Download."

## Walkthrough: from idea to merged change

This example follows one idea through every step.
Each step shows what you type and what happens next.

### 1. Decide

```text
<skill start form for grill-with-docs> Members want a reminder the day before each booking.
```

The agent asks questions, each with its recommended answer.
Reply "yes" to take the recommendation, or answer in your own words.
"I do not know, what do you recommend?" is a good answer.
The agent finds facts in the code itself; it asks you only for Decisions.
It records each Decision in an ADR or the issue, and each new word in `GLOSSARY.md`.
It stops when nothing is left to decide, and asks you to confirm.

### 2. Write the Spec

Stay in the same session, because `to-spec` writes from the conversation so far.

```text
<skill start form for to-spec>
```

The agent asks you to confirm where the tests touch the code.
Then it publishes one Spec issue and gives you its link.
Approving a Spec does not start the build.

### 3. Split it into Tickets

```text
<skill start form for to-tickets> <link to the Spec>
```

The agent shows the breakdown: each Ticket, its Expected Tier (the risk class from 0 to 4), and which Ticket blocks which.
Ask for changes until the breakdown is right, then approve it.
The agent publishes the Tickets.

### 4. Build

For the whole Spec:

```text
<skill start form for implement-spec> <link to the Spec>
```

For one Ticket:

```text
<skill start form for implement> <link to the Ticket>
```

This request is your permission to build.
The agent runs the design and proof skills that each Ticket's Expected Tier needs, so you do not start them yourself.
When a Ticket needs a new Decision outside your Standing Decisions ADR, the agent stops and asks you.
For Tier 0 to 2 work, you may also give one upfront go-ahead from Spec through Build (for example "Write the Spec, split it, and build it if every Ticket stays Tier 2 or lower"); the agent still stops if any Ticket rises to Tier 3 or needs a new Decision.

### 5. Land

The agent opens a pull request with an Evidence bundle: the proof that the change works.
<What the Owner does at merge time in this Review mode, by Tier, in plain words.>
A Tier 4 change never runs alone: you start it, or you watch it run.

## Other ways in

### Work too big for one session

```text
<skill start form for wayfinder> Move every member's data to the new account model.
```

The first session charts a Map and stops.
Each later session settles one Decision on the Map:

```text
<skill start form for wayfinder> <link to the Map>
```

When the Map has no open Decisions, run `to-spec` in that session.

### A bug report or a feature request

```text
<skill start form for triage> <link to the issue>
```

With no link, the agent shows the issues that wait for triage.
An issue marked `ready-for-agent` goes straight to `implement`.

### A repeated agent mistake

```text
<skill start form for correction> Agents run the full test suite before every commit. See <link> and <link>.
```

The agent opens a `correction` issue with one example that the new check must reject and one it must accept.
Ask it to build the check when you want the mistake to stop for good.

## Skills that you do not start

The skills above load these helpers themselves, so you never start them:

- `grilling`, `domain-modeling`, `research`, and `prototype`
- `tdd` and `codebase-design`
- `in-your-own-words`, `how`, `why`, and `unslop`
- `principle-separate-before-serializing-shared-state`, `principle-redesign-from-first-principles`, `principle-prove-it-works`, `principle-laziness-protocol`, and `principle-encode-lessons-in-structure`

## What you approve, and when

- Each Decision outside your Standing Decisions ADR, during `grill-with-docs`, `wayfinder`, or a build that stops to ask.
- Any change to your Standing Decisions ADR (always Tier 3).
- The Ticket breakdown before `to-tickets` publishes it, and the start of each build by starting `implement` or `implement-spec` (or one upfront go-ahead for Tier 0 to 2 Spec through Build).
- Each merge that the Review mode and active autonomy gate give to you.
- Each Tier 4 operation, which you start or watch.

## Setup on your machine

- On a new machine, restore the Project's skills with `<skills restore command>`.
- `arena` and `interrogate` start agents on several models.
  To choose those models, run pstack's `setup-pstack` skill once in your own agent setup.
  Without it, they use their built-in defaults.

## When something goes wrong

- An agent says "run setup-matt-pocock-skills".
  Do not run it.
  Tell the agent: "Read the Agent skills block in `.rules`."
- An agent says it cannot start a skill.
  Tell it: "Read `<skills folder>/<skill name>/SKILL.md` and follow it."
- An agent's message makes no sense.
  Start `wait-what`.
- To move to a newer Kit version, tell an agent: "Upgrade this project from https://github.com/avidianity/factory. Read UPGRADE.md there and follow it."
````

## Fill-in rules

- Fill the agent table with one row per agent that the Project uses, from [adapters/agents.md](../adapters/agents.md#how-the-owner-starts-a-skill).
- Write every sample in the form of the first agent in the table.
- Fill the Review mode lines from [agents/review-and-merge.md](./agents/review-and-merge.md#mandatory-review-modes), in plain words.
- Add one row to "Pick a skill" for each Project skill.
  Remove a row only for a skill that the Project removed in an ADR.
- Keep every skill name in the Project's skills folder on this page.
  The check spec [skills-in-owner-guide](../checks/skills-in-owner-guide.md) enforces it.
- Write for an Owner who does not read code, in every Review mode.
