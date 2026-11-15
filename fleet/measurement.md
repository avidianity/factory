# Measurement

The light records in each Project tell whether agents are safe.
This page adds how to tell whether they are worth it.
Use it when the Owner wants to compare ways of working, or before adding Workers.

## What to measure

Count accepted outcomes, not activity.

| Metric | Definition |
| ------ | ---------- |
| Accepted tasks | Tasks merged and not reverted within 14 days |
| Cycle time | From task start to merge, median and 90th percentile |
| Owner time | Minutes of Owner attention per accepted task: reviews, answers, fixes |
| Escaped-defect score | Sum of severity weights of Escaped defects, per 10 accepted tasks |
| Rework rate | Share of merged tasks that needed a follow-up fix within 14 days |
| Cost | Model spend plus CI minutes per accepted task |

Severity weights:

| Severity | Weight |
| -------- | ------ |
| S1 critical | 16 |
| S2 major | 8 |
| S3 moderate | 3 |
| S4 minor | 1 |

Never count pull requests, lines of code, or tokens as output.
An agent can raise each of those without helping anyone.

## Comparing two ways of working

Run a small trial before you change the whole Fleet.

1. Pick a change to test, such as "Independent reviewer on Tier 1" or "four Workers instead of two".
2. Split comparable tasks into two arms: the current way and the new way.
   Match task kinds and sizes across arms.
3. Run at least 20 tasks per arm.
4. Compare all metrics together.
   A faster arm with a higher escaped-defect score is not better.
5. Decide in advance which result keeps the change, and which reverses it.

## Cost model

Per accepted task:

```text
cost = model spend + CI minutes x CI price + Owner minutes x Owner rate
       + escaped-defect score x cost per weighted defect
```

The Owner sets the rates.
The Owner's own time is usually the largest term, so a change that saves Owner time can be worth more model spend.

## Reading the numbers

- Small samples swing.
  Treat a difference under about 20 percent across 20 tasks as noise.
- Escaped defects appear late.
  Wait 14 days after the last task before you close a trial.
- When the slowest shared stage is full, more Workers raise cycle time.
  Look at queue times before you blame the Workers.
