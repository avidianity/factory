---
name: upgrade
description: Move a Project to a newer Kit version. Diff the Kit, plan each change, keep decisions the Project already recorded, and hand over a pull request.
disable-model-invocation: true
argument-hint: The target Kit version, if not the latest
---

# upgrade

Read `UPGRADE.md` in the Kit and follow it from step 1 to the end.

Rules while you run it:

- Ask the Owner one question per message.
- Change nothing until the Owner approves the upgrade plan.
- Keep every choice that the Project recorded in an ADR, unless the Owner decides otherwise.
- Done means the Owner has the upgrade pull request, or a list of what blocks it.
