# Review prompts

These prompts start reviews that do not trust the author.
Give each one to a fresh agent session that did not write the change.
Give it the code and the task, not your own conclusion.

## Independent verifier

Use this for the Independent reviewer that [core/agents/review-and-merge.md](../core/agents/review-and-merge.md) requires.

```text
You are an independent verifier. You did not write this change.
Inputs: the task contract, the diff, and the Evidence bundle.

1. Restate the acceptance criteria in your own words.
2. For each criterion, find the test or runtime evidence that proves it.
   Run each test yourself on the exact revision. Do not trust the reported result.
3. Check that each fail-first test fails without the change and passes with it.
4. Look for changes that weaken checks: deleted or skipped tests, changed expected
   values, suppressions, raised timeouts, or CI edits.
5. Look for scope creep: files changed outside the task's allowed paths.
6. For a user-visible change, check that the evidence includes the stored effect,
   not only a screenshot, and that it comes from this revision.

Verdict: PASS, FAIL, or INCONCLUSIVE.
For FAIL, list each finding with file, line, and a concrete failing scenario.
For INCONCLUSIVE, name what evidence is missing.
Never approve on the author's explanation alone.
```

## Adversarial review of a guidance change

Use this before you add or change agent instructions or checks.

```text
Review this proposed agent instruction or check as a skeptical engineer.
Do not accept it because it sounds plausible.

1. Identify the exact failure it claims to prevent.
2. Name the evidence that this failure happens in this Project.
3. Explain how an agent could still fail while it follows the rule.
4. Find the cheapest lower layer that could enforce it: design, database,
   type, lint, test, or runtime verification.
5. Estimate false positives and the cost of keeping it current.
6. Find conflicts with existing rules.
7. Propose one failing case and one passing case.
8. Say when to remove it.

Verdict: adopt, revise, run a small trial first, or reject.
```

## State-machine challenge

```text
Here is a proposed state model, with its invariants.
Try to break it.

1. Find a state the code can reach that the model does not include.
2. Find a transition the code allows that the model forbids, or the reverse.
3. Find a retry, timeout, crash, or concurrent request that breaks an invariant.
4. Find an assumption the model makes that production does not guarantee.
5. For each finding, write the smallest executable test that would show it.

Report findings ordered by user impact. Do not propose fixes yet.
```

## Verification-skill attack

Use this to prove that a runtime verifier catches real failures.

```text
You are given a Feature map and the Verification contract.
Your goal is to make the verifier report PASS while the feature is broken.

Try, one at a time:
- break the stored effect while the screen still looks right;
- serve an old build;
- reuse evidence from an earlier run;
- run as the wrong role;
- remove a negative path from the Feature map;
- let Cleanup delete the evidence.

For each attempt, report whether the verifier caught it.
Every attempt it misses is a finding.
```

## Measurement review

```text
Review this proposal to measure agent work.

1. Does each metric measure accepted outcomes, not volume?
2. Can an agent improve the metric without improving the product?
3. Is there a baseline from before the change?
4. Is the sample big enough, and are task types matched?
5. Are cost, cycle time, and escaped defects counted together?
6. Which result would make us stop or reverse the change?

Verdict: sound, fixable (list fixes), or misleading.
```

## Kill test for an existing rule

```text
Here is an existing rule or check. Argue for deleting it.

1. Has it caught anything in its last 30 relevant changes? Show the records.
2. Does a lower layer now cover the same failure?
3. What does it cost per change: time, false positives, confusion?
4. What would we lose if it were gone tomorrow?

Verdict: keep, shrink, move to a lower layer, or delete.
```
