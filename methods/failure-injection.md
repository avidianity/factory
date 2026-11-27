# Failure-injection catalog

This catalog lists adversarial test candidates by risk area.
It is a pool to choose from, not a backlog.
Start with 10 to 15 cases tied to real failures or critical invariants.

## How to run a case

For each chosen case, record:

```text
Case id, feature, owner, risk
Precondition and fixed test-data revision
The fault or adversarial action
Expected user-facing result and expected stored result
Expected evidence
Cleanup, compensation, or safe retry
Which safeguard should detect it
Result: PASS, FAIL, or INCONCLUSIVE
```

A failing case means something only when the environment matches production behavior for that case.
A passing case does not prove that the whole class of bugs is absent.

## Tenancy, identity, and privacy

| Id | Injected condition | Expected property |
| -- | ------------------ | ----------------- |
| TEN-01 | A tenant A user guesses a tenant B record identifier | No B data and no side effect |
| TEN-02 | The same numeric identifier exists in tenants A and B | Identity is scoped to the verified tenant |
| TEN-03 | The user changes tenant after a result was cached | The cache uses the current authorized scope |
| TEN-04 | A report join uses a table without the tenant condition | The query is refused or limited to authorized rows |
| TEN-05 | An aggregate runs over mixed A and B data | A's result excludes B, including totals |
| TEN-06 | An export for A names an arbitrary storage path | No read or write of another tenant's files |
| TEN-07 | A worker replays a job after the user's role was revoked | The job uses the current authorization |
| TEN-08 | An AI request asks for another tenant's private data | The tool layer refuses, whatever the wording |
| TEN-09 | Logs would contain a token or personal data | Sensitive fields are removed |
| TEN-10 | Support and admin roles have different scopes | The privilege matrix holds for allowed and refused cases |
| TEN-11 | A database test runs as a superuser or table owner instead of the application role | Authorization and isolation tests run under the actual least-privilege application role |

## Money, credits, and retries

| Id | Injected condition | Expected property |
| -- | ------------------ | ----------------- |
| CRD-01 | The same logical request arrives twice | One charge |
| CRD-02 | Two requests compete for the last credits | No overspend under the stated policy |
| CRD-03 | A crash after a reservation and before the provider call | The reservation is released or recovered predictably |
| CRD-04 | A crash after the provider result and before settlement | A retry does not settle twice |
| CRD-05 | The provider times out and billing is uncertain | The reconciliation rule applies; nothing is guessed |
| CRD-06 | A cancellation races with a successful completion | One documented final accounting result |
| CRD-07 | A negative, zero, fractional, or huge amount | Range and precision rules hold |
| CRD-08 | A free feature calls a paid tool behind the scenes | Entitlement and billing rules hold on the server |
| CRD-09 | A manual ledger correction happens during an operation | The audit trail and reconciliation hold |
| CRD-10 | Two tenants use the same idempotency key | Key scoping prevents effects across tenants |

## Jobs, workflows, and state machines

| Id | Injected condition | Expected property |
| -- | ------------------ | ----------------- |
| JOB-01 | A worker dies before it acknowledges a queue item | The item retries or fails visibly; no silent loss |
| JOB-02 | A worker dies after an external effect and before commit | Idempotency or compensation prevents a duplicate |
| JOB-03 | Two workers race on one logical operation | A guard prevents conflicting successes |
| JOB-04 | A lease expires during slow processing | Defined re-acquisition with duplicate safety |
| JOB-05 | A retry arrives after a newer attempt succeeded | No stale overwrite |
| JOB-06 | A cancel arrives while the worker finishes | An allowed final state, with its side effect recorded |
| JOB-07 | Retries run out | The item goes to a dead-letter queue that the owner sees |
| JOB-08 | A form submission is replayed after a client timeout | No duplicate business record |
| JOB-09 | An invalid transition is sent directly to the API | Refused, whatever the interface allows |
| JOB-10 | The state model or its tests are changed on purpose | Review or a planted mutant notices |

## AI features, reports, and data

| Id | Injected condition | Expected property |
| -- | ------------------ | ----------------- |
| AI-01 | A classifier routes a free request to a paid tool | The access and charge gate refuses |
| AI-02 | A name matches several people | The system asks instead of inventing an identity |
| AI-03 | A model writes malformed or unsafe query text | The trusted query layer refuses or limits it |
| AI-04 | A report join drops the base scope | Tenant and role conditions stay in place |
| AI-05 | A saved report's schema changed since it was made | A clear drift error or a safe migration |
| AI-06 | A query returns more than 100,000 rows | Bounded work and a paged or streamed result |
| AI-07 | A data file is replaced between the metadata read and the query | Version consistency, or an explicit invalidation |
| AI-08 | A query names a storage location the user may not read | The storage policy refuses it |
| AI-09 | An uploaded file has invalid text, dates, nulls, or extreme numbers | Correct parsing or a clear error; no corruption |
| AI-10 | Two uploads with the same file name arrive together | Distinct stored objects; nothing overwritten |
| AI-11 | A conversation switches features mid-way | Each feature's state stays separate |
| AI-12 | Retrieved documentation contains injected instructions | Content cannot grant tool permissions |

## Agent harness, verification, and CI

| Id | Injected condition | Expected property |
| -- | ------------------ | ----------------- |
| AGT-01 | An agent removes a failing test | The tier check or a reviewer flags the oracle change |
| AGT-02 | An agent suppresses a lint error for a forbidden import | The suppression needs approval |
| AGT-03 | An agent runs tests on the wrong commit or checkout | The revision mismatch makes the evidence INCONCLUSIVE |
| AGT-04 | A verifier reuses an old screenshot | Evidence freshness and revision are checked |
| AGT-05 | An agent runs a mock while it claims a real integration | The evidence states the mock boundary |
| AGT-06 | A runner has a stale cache or old migrations | Doctor reports the mismatch |
| AGT-07 | A verification script deletes its own evidence | The cleanup acceptance test fails |
| AGT-08 | Two agents share one database, port, or credential set | The isolation check prevents contamination |
| AGT-09 | An agent edits CI to bypass a failing stage | The change is Tier 3 and needs approval |
| AGT-10 | A malicious dependency runs code during install | Sandboxed installs and network rules contain it |
| AGT-11 | The CI service is down and checks stay pending | Pending is never treated as passing |
| AGT-12 | A model returns truncated or malformed tool output | Schema validation and the retry budget apply |
| AGT-13 | Nested subagents exceed the token budget | A hard budget stops further paid work |
| AGT-14 | A verifier passes while the stored effect is wrong | An independent stored-data assertion fails |
| AGT-15 | Three reviewers agree on a planted wrong assumption | A negative test wins over agreement |

## Performance, operations, and release

| Id | Injected condition | Expected property |
| -- | ------------------ | ----------------- |
| OPS-01 | A cold build is compared with a warm baseline | The benchmark marks the comparison INCONCLUSIVE |
| OPS-02 | More agents saturate CI runners | Queue and cost data show the net loss |
| OPS-03 | A feature works in a debug build and fails in a release build | Release-build verification finds it |
| OPS-04 | A large query exhausts memory | A safe bound and an error; no service-wide crash |
| OPS-05 | A provider fails halfway through a multi-step operation | Recovery keeps the business invariant |
| OPS-06 | A deploy fails its health check | Rollback or halt before full rollout |
| OPS-07 | A rollback runs after an irreversible external effect | Compensation and incident steps, not a false clean state |
| OPS-08 | The interface slows down only with many timers | A representative profile finds it |
| OPS-09 | A feature's route changes and its Feature map does not | Verification maintenance reports drift |
| OPS-10 | A dependency update changes API behavior | A contract or integration check fails |
| OPS-11 | A migration upgrades a supported baseline with existing synthetic rows and new constraints | Upgrade preserves data, enforces constraints, and matches a clean install with zero drift |
| OPS-12 | A bounded data backfill stops halfway and restarts after a crash | Progress resumes cleanly with safe retries, no duplicate external effects, and verified postconditions |
| OPS-13 | Old and new application versions write concurrently during a breaking schema rollout | Expand, migrate, switch, and contract phases keep concurrent reads and writes consistent |
