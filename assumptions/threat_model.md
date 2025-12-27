# Threat Model

This document defines the threat model addressed by the DBL architecture.
The goal is to specify which classes of failures and attacks are structurally
mitigated, and which are explicitly not addressed.

---

## Assets

The primary assets protected by the model are:

- Integrity of normative decisions
- Auditability of governance behavior
- Replayability of normative state
- Clear attribution of responsibility

The model does not aim to protect execution outcomes themselves.

---

## Adversary Capabilities

The adversary may:

- Provide adversarial or malformed inputs (e.g. prompt injection)
- Exploit non-deterministic behavior of effectors
- Observe system outputs and execution traces
- Trigger repeated executions with identical inputs
- Attempt to influence decisions indirectly via observational feedback

The adversary is not assumed to have:
- write access to the event stream V
- the ability to modify policy code or boundary definitions at runtime

---

## Threats Addressed

### T1. Prompt Injection and Input Manipulation
Mitigated by:
- strict boundary-controlled admission of authoritative inputs
- governance decisions made prior to execution
- explicit DECISION events

---

### T2. Observational Feedback Loops
Mitigated by:
- prohibition of observational data in I_L
- invariant G/L separation
- explicit admission requirements for any observational signal

---

### T3. Non-Deterministic Decision Drift
Mitigated by:
- deterministic policy evaluation
- append-only, replayable event stream
- separation of governance from execution

---

### T4. Audit Evasion
Mitigated by:
- explicit DECISION events
- replay equivalence of normative state
- absence of hidden normative state

---

## Threats Not Addressed

The following threats are explicitly not mitigated:

- Compromised execution environment
- Side-channel leakage via timing, power, or resource contention
- Malicious or incorrectly authored policies
- Attacks on external dependencies
- Data poisoning outside the boundary admission process

---

## Failure Interpretation

If a mitigated threat succeeds in practice, this indicates a violation of one
or more model assumptions and must be treated as an implementation error or
boundary misconfiguration, not as a limitation of the theoretical model.
