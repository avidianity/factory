# Upgrade

This playbook moves a Project from the Kit version it recorded to a newer Kit version.
An agent follows it from start to end.
Upgrade never undoes a choice that the Project already made on purpose.

## Rules for the agent that upgrades

The rules in [BOOTSTRAP.md](./BOOTSTRAP.md#rules-for-the-agent-that-bootstraps) apply here too.
In addition:

- An ADR that settled a Stack-convention conflict or a Kit rule wins over the new Kit text, unless the Owner decides otherwise.
- A Project rule that is stricter than the Kit stays.
- Upgrade changes guidance and checks only.
  It never changes application code.

## Step 1: find the two versions

1. Read the Kit version line in the Project's `.rules`.
   It names the version that the Project last bootstrapped or upgraded to.
2. Pick the target: the version the Owner named, or the newest release tag of the Kit.
3. When the two are equal, tell the Owner that the Project is current, and stop.

Done when you have the old version and the new version, as tags or commit ids.

## Step 2: read what changed in the Kit

1. Read the Kit's release notes between the two versions, when they exist.
2. Diff the Kit between the two versions, file by file.
3. Sort each change into one kind:
   - **New**: a new page, check spec, template, or skill.
   - **Changed**: a changed rule, check spec, template, or skill.
   - **Removed**: a page, rule, or check spec the Kit no longer has.
   - **Wording**: a change that does not change any rule.
4. Map each change to the Project file it affects, with the table in [BOOTSTRAP.md](./BOOTSTRAP.md#step-5-write-the-project-files).

Done when every changed Kit file has a kind and a Project target.

## Step 3: survey the Project

Run the Survey from [BOOTSTRAP.md](./BOOTSTRAP.md#step-2-survey-the-project), and add:

- every ADR that mentions the Kit, a Kit rule, or a Stack-convention conflict;
- every change the Project made to its copies of Kit pages since the last upgrade, from Git history;
- every check that the Project built from a Kit check spec, and whether it still passes both ways;
- open Corrections and Escaped defects that touch a changed Kit rule.

The Survey stays read-only.

## Step 4: decide each change

For each change from step 2:

| Situation | What to do |
| --------- | ---------- |
| Wording only | Apply it, unless the Project rewrote that text |
| New rule or check, no conflict | Plan to add it |
| Changed rule, the Project's copy is untouched | Plan to apply it |
| Changed rule, the Project changed its copy | Ask the Owner: keep the Project's version, take the Kit's, or merge |
| Change conflicts with an ADR | Keep the ADR's choice; tell the Owner, and ask only when the reason in the ADR no longer holds |
| Removed rule | Ask the Owner whether to remove it from the Project |
| New check spec | Plan to build it, under step 6 of [BOOTSTRAP.md](./BOOTSTRAP.md) |
| New or changed pinned skill | Plan to install it at the hash in [skills/README.md](./skills/README.md) |
| Removed Kit skill | Ask the Owner whether to delete the Project's copy |

Ask one question per message, with your recommendation.

## Step 5: write the upgrade plan

Use [templates/adoption-plan.md](./templates/adoption-plan.md), titled "Upgrade plan for <Project>: <old version> to <new version>".
List each change, its action, and each answer the Owner gave.

The Owner approves the plan before you change anything.

## Step 6: apply the plan

1. Work on a new branch, under the Project's Git rules.
2. Apply each planned change.
3. Build each new or changed check, and prove that its failing case fails and its passing case passes.
   Two attempts per check; then record it as open.
4. Record each new Owner answer about a conflict as an ADR.
5. Update the Kit version line in `.rules` to the new version and today's date.
6. Run the Project's checks for guidance: doc size, doc links, and agent entry links.

## Step 7: prove the setup

Start a fresh agent session in the Project, and ask it the five questions from [BOOTSTRAP.md](./BOOTSTRAP.md#step-7-prove-the-setup).
Add one question for each changed rule: "What does the Project require for <topic>?"
Fix the guidance until every answer is right.

## Step 8: hand over

1. Open one pull request with all changes.
   Changes to guidance and checks are Tier 3.
2. Fill the Evidence bundle, and list each changed check with its proof.
3. Write a Risk summary when the Owner does not read code.
4. Report the pull request URL, and every open item.

Done when the Owner has the pull request, or a list of what blocks it.
