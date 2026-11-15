# Watch and release

Workers get stuck.
The Coordinator finds stuck Workers fast, and releases finished ones at once.

## Watching

Run a watchdog: a small script that reads each Worker's screen or status on a timer, at 60 seconds or slower, and reports one of these states:

| State | Meaning | Action |
| ----- | ------- | ------ |
| WORKING | The Worker is producing output | None |
| WAITING-ON-ANSWER | The Worker asked a question or shows a dialog | Answer from the decision log, or ask the Owner |
| UNSENT-INPUT | Text sits in the Worker's input box but was never sent | Send it, or clear it |
| IDLE | The Worker finished its turn and waits | Read its report; release it or give it the next step |
| FROZEN | No change for longer than the slowest normal step | Nudge it |

Prove each detector before you trust it.
Make a Worker enter each state on purpose, and confirm that the watchdog reports it.

## Unsticking

Try these in order, and stop at the first that works:

1. Send a short nudge: "Status? Continue the task, or report what blocks you."
2. Press Escape to interrupt a stuck tool call, then nudge again.
3. Restart the Worker with a fresh session and an exact handoff of what the old one finished.

Never resume a stuck session's context into a new task.

## Releasing

A finished Worker is released at once:

1. Ask it to exit, or close its session.
2. Close its terminal tab or window.
3. Return its isolated checkout, from outside that checkout.
4. Remove its branch when the branch has merged.

Keep the checkout of a Worker whose work is unmerged, blocked, or waiting for review.

## Hygiene

- Check free disk space every day.
  Checkouts, build caches, and evidence files grow fast with many Workers.
- Recycle long-running sessions every day: start a fresh one with a handoff, because long contexts drift.
- Keep a list of every live Worker, its task, and its checkout.
  Compare it with what is actually running, and close what is not on the list.
