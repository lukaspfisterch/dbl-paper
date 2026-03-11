# Case Study: Financial Transaction

## Scenario

A payment system processes transfer requests between accounts.
Execution depends on external settlement infrastructure (banking APIs,
network latency, partial failures) and is inherently non-deterministic.
Regulatory requirements demand that every approval or denial is auditable
and reconstructible independently of settlement outcomes.

## Event Trace

```
INTENT:    transfer 100 CHF from Account A to Account B
DECISION:  APPROVE (policy v3: amount within limit, accounts active)
EXECUTION: settlement initiated via banking API
PROOF:     confirmation code BC-2024-7891, settled in 340ms
```

## Boundary Admission

L extracts: sender, recipient, amount, currency, account status flags.
Excluded: network latency, settlement queue depth, previous transaction timing.
Structural validation failure → pre-ontological rejection, no INTENT written.

## Governance Decision

G evaluates I_L under p_v (transfer limits, compliance rules, account restrictions).
DECISION (APPROVE or DENY) appended to V before settlement.
Decision depends only on (I_L, p_v), not on infrastructure availability.

## Execution and Observation

Settlement via external banking infrastructure only if APPROVE.
Result (success, timeout, partial failure), confirmation codes, timestamps
recorded as EXECUTION and PROOF events. Observational only.

## Replay

Auditor replays V → same DECISION sequence from (V, I_L, C, p_v).
No access to settlement infrastructure or network conditions required.
Whether original settlement succeeded or failed: irrelevant to normative record.
