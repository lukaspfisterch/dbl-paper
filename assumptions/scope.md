# Scope and assumptions

This document defines the scope for the claim set and labels the minimal assumptions used by the proofs. It is normative for what the claims do and do not cover.

## Assumptions (A1–A5)

- **A1 (Append-only V):** The event stream V is append-only and events are immutable once written.
- **A2 (DECISION primacy):** All normative effects are represented only by explicit DECISION events in V.
- **A3 (Authoritative inputs):** Governance consumes only the authoritative input set I_L admitted by L; observational data is excluded.
- **A4 (Deterministic governance):** For fixed authoritative inputs and fixed policy configuration, governance G deterministically produces DECISION events.
- **A5 (Pre-execution decision):** DECISION events are written before any corresponding execution.

## Out of Scope

- Correctness or optimality of policies.
- Determinism of effector outputs.
- Performance, latency, or availability guarantees.
- Side-channel resistance beyond the explicit admission rules.
- Completeness or accuracy of observations.

## Interpretation Rule

If an observation influences a decision without explicit admission into I_L, the system violates the model and the claims do not apply.
