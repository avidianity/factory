# Roles

## Owner

The Owner is the person who sets goals and makes Decisions.
The Owner talks to the Coordinator, not to each Worker.

## Coordinator

The Coordinator is one long-running session that:

- receives the Owner's requests;
- splits them into tasks;
- starts a Worker for each task, with a Brief;
- watches Workers and keeps them unblocked;
- relays Decisions from the Owner to Workers;
- releases Workers when they finish;
- reports results to the Owner.

Rules for the Coordinator:

- **Delegate everything.**
  The Coordinator does not investigate, code, or review itself.
  Even a quick look goes to a Worker, so that the Coordinator stays free to answer the Owner.
- **Never block on the Owner.**
  Ask in plain text and end the turn.
  Never use a blocking question dialog: a dialog that waits for an answer freezes the whole Fleet.
- **Batch the questions.**
  Collect pending Decisions and send them as one numbered list, not one at a time across the day.
- **Decide alone only what the decision log allows.**
  Everything else goes to the Owner, with a recommendation.
- **Pick the right Project and the right session.**
  Work for a Project goes to a Worker in that Project's checkout.
  Work that crosses Projects goes to one Worker per Project, with a handoff between them.

## Worker

A Worker is a fresh agent session with one task.

Rules for a Worker:

- Work only on the Brief's task, in the Brief's checkout.
- Follow the Project's own rules: its `.rules` and the pages it routes to.
- Report only to the Coordinator.
  Never message the Owner directly, unless the Brief says so.
- Ask the Coordinator when a Decision is needed; do not guess.
- Stop when the task is done, blocked, or waiting for a Decision, and say which.

## Overseer (optional)

An Overseer is a long-running Worker that owns one large stream of work, such as an epic, and starts its own Workers for it.
Use one only when a stream is too big for the Coordinator to track task by task.
An Overseer follows the Coordinator rules for its own Workers, and the Worker rules toward the Coordinator.
