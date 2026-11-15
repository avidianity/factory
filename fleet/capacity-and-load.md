# Capacity and load

More Workers help only while the machine, the shared budgets, and the Owner's attention have room.

## The cap

- Set a Worker cap from the machine's memory headroom, not from the number of tasks.
  Measure the memory of a typical Worker with its checkout, tests, and tools running.
  Keep at least a quarter of memory free under that load.
- Reserve one slot for urgent work, such as a production bug.
  Normal work never takes the reserved slot.
- When the cap is reached, queue new tasks.
  Do not start more Workers.

## Heavy jobs

Builds, full test suites, browser runs, and large installs load the machine for minutes.

- Run heavy jobs in lanes: at most a set number at the same time, across all Workers.
- A Worker waits for a free lane before it starts a heavy job.
- Prefer CI for whole-suite runs; run only the tests that cover the change on the machine.

## CI

- Workers do not poll CI.
  The session that publishes a pull request watches its run, at 60 seconds or slower.
- All sessions on one forge account share one API budget.
  Check the remaining budget before a bulk loop, and stop below a safe remainder.

## Model usage

- When several model accounts or providers are available, spread work so that each one runs out near its own reset, not early.
- Use the cheapest model that does the task well.
  Record the model ladder (which model for which kind of task) in the decision log.
- Never use a faster, more costly tier by default.

## Signs of overload

Stop starting Workers, and tell the Owner, when you see:

- memory pressure or swapping;
- commands that time out that used to pass;
- CI queues longer than the time a run takes;
- review or merge queues that grow every hour.
