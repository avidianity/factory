# Evidence bundle template

Use this as the pull request body.
The check spec [evidence-bundle](../checks/evidence-bundle.md) requires each section.

````markdown
## Summary

<What changed and why, in two or three sentences.>

Closes #<issue>

## Tier

Tier <n> from the tier check: <the rule that matched>.

## Task

<Link to the task, and its acceptance criteria as a checklist.>

## Fail-first test

- Test: `<test name or path>`
- Before the change: <failed, with the decisive line of output>
- After the change: passed

<Or: "No behavior change: <reason>.">

## Checks

| Check | Result |
| ----- | ------ |
| <relevant tests> | <passed / failed> |
| <hook checks> | <passed> |
| Merged-result check | <passed> |

## Safety proof

- Safety fact: <the one fact the change is safe because of>
- Proof: <the script or test that ran the real code, and its decisive output; or "unproven">
- `interrogate`: <none run below Tier 3; or each verified finding, its fix, and the clean rerun>

<Or: "Not applicable: Tier 0 or 1.">

## Runtime evidence

<For a user-visible change: the feature map, the verdict, the build revision, and links to traces or screenshots.>

<Or: "Not applicable: no user-visible change.">

## Gaps

- Deferred coverage: <none, or what and why>
- Pre-existing failures: <none, or the needs-triage issue>
- Environmental failures: <none, or what>
````
