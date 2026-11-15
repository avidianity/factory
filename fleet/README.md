# Fleet

The Fleet is an optional way of working: one Coordinator session that delegates to many Worker sessions, across one or more Projects.
A Project needs none of it.
Bootstrap does not copy these pages into a Project; the Owner keeps them in their own agent setup.

Read the Fleet pages when the Owner runs more than a few agent sessions at the same time.

| Page | Covers |
| ---- | ------ |
| [roles.md](./roles.md) | The Coordinator and Worker roles, and who does what |
| [briefs.md](./briefs.md) | How the Coordinator briefs a Worker, and how Workers report |
| [capacity-and-load.md](./capacity-and-load.md) | How many Workers can run, and how to keep the machine and shared budgets healthy |
| [watch-and-release.md](./watch-and-release.md) | How to find stuck Workers, and how to release finished ones |
| [decision-log.md](./decision-log.md) | Where the Owner's Decisions live, so that every session reads the same answer |
| [promotion-gates.md](./promotion-gates.md) | How agent autonomy grows step by step |
| [measurement.md](./measurement.md) | How to measure whether the Fleet helps |
| [personal-profile.md](./personal-profile.md) | The Owner's communication preferences, as an example to copy |

## When the Fleet is worth it

The Fleet pays off when the Owner has more independent tasks than one session can carry, and the slowest shared stage (review, CI, or the Owner's own attention) still has room.
When that stage is full, more Workers add queue time, not finished work.
Start with one Coordinator and two Workers, and add Workers only when the records in [measurement.md](./measurement.md) show that it helps.
