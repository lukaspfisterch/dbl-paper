# DBL Paper Workspace

## Purpose

This repository is a claim-driven scaffold for a paper on Deterministic Boundary Layers (DBL). DBL defines a deterministic boundary for governed execution of non-deterministic systems, with normativity expressed only as explicit DECISION events in the append-only event stream V. Governance (G) and Boundaries (L) are strictly separated. The focus is replayability and auditability of normative state.

## Repository layout

- [assumptions/scope.md](assumptions/scope.md)
- [assumptions/threat_model.md](assumptions/threat_model.md)
- [claims/01_claim_1.md](claims/01_claim_1.md)
- [claims/02_claim_2.md](claims/02_claim_2.md)
- [claims/03_claim_3.md](claims/03_claim_3.md)
- [claims/04_claim_4.md](claims/04_claim_4.md)
- [proofs/01_deterministic_governance.md](proofs/01_deterministic_governance.md)
- [proofs/02_gl_invariance.md](proofs/02_gl_invariance.md)
- [proofs/03_replay_equivalence.md](proofs/03_replay_equivalence.md)
- [proofs/04_observational_noninterference.md](proofs/04_observational_noninterference.md)
- [related_work/](related_work/)
- [case_study/](case_study/)
- [paper/](paper/)
- [notes/](notes/)

## Reading order

1) assumptions -> claims -> proofs -> threat model -> case study -> related work -> paper

## Status

- Claim set frozen.
- Proof sketches complete.
- Drafting in progress.

## Consistency rule

Claims apply only under the stated assumptions; observational data must not influence decisions without explicit admission.
