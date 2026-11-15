# Promotion gates

Agent autonomy grows in steps.
Each step opens only when the records show the previous step is safe.
The records are the light ones from [core/agents/corrections.md](../core/agents/corrections.md): Escaped defects, Corrections, and Evidence bundles.

## Gate 0: assisted

Agents propose changes.
The Owner reviews and merges every change.

To move to Gate 1, all of these hold:

- The Project's checks are built and proven both ways.
- The tier check labels every pull request.
- 20 agent pull requests merged, each with a complete Evidence bundle.

## Gate 1: agents merge Tier 0 and 1

Agents merge Tier 0 and Tier 1 changes alone, under the merge contract.
The Owner approves Tier 2 and above.

To move to Gate 2, all of these hold:

- 30 agent merges at Tier 0 and 1.
- No Escaped defect of severity S1 or S2 from them.
- Every Escaped defect of severity S3 or S4 has a Correction or a merged check.

## Gate 2: agents merge Tier 2 with an Independent reviewer

Agents merge Tier 2 changes after an Independent reviewer passes them.
The Owner approves Tier 3 and 4.

To move to Gate 3, all of these hold:

- 30 agent merges at Tier 2.
- No Escaped defect of severity S1 or S2 from them.
- The Independent reviewer caught at least one real problem in a planted test.

## Gate 3: steady state

Agents merge Tier 0 to 2 alone.
Tier 3 needs the Owner's Decision, with a Risk summary when the Owner does not read code.
Tier 4 always needs the Owner.

The Review mode the Owner chose at Bootstrap limits these gates.
Under Review mode "Always", the Project stays at Gate 0 whatever the records say.

## Stop triggers

Any stop trigger in [core/agents/corrections.md](../core/agents/corrections.md#mandatory-stop-triggers) drops the Project to Gate 0 at once.
It climbs again from Gate 0, with fresh counts.
