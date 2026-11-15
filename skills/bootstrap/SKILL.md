---
name: bootstrap
description: Set up a Project with the factory Kit. Survey, interview the Owner, plan, write rules and checks, prove them, and hand over a pull request.
disable-model-invocation: true
argument-hint: Path or URL of the Kit, if not the default
---

# bootstrap

Read `BOOTSTRAP.md` in the Kit and follow it from step 1 to step 8.

Rules while you run it:

- Ask the Owner one question per message.
  Give your recommended answer with each question.
- Change nothing in the Project until the Owner approves the Adoption plan.
- Never overwrite an existing file.
  Merge into it, or ask.
- When the Project's stack convention conflicts with a Kit rule, stop, ask the Owner, and record the answer in an ADR.
- Done means the Owner has the setup pull request and its Risk summary, or a list of what blocks it.
