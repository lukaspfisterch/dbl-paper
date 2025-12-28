# Deterministic Boundary Layers (DBL)

[![build-paper](https://github.com/lukaspfisterch/dbl-paper/actions/workflows/build-paper.yml/badge.svg)](https://github.com/lukaspfisterch/dbl-paper/actions/workflows/build-paper.yml)

## What this paper claims

Deterministic Boundary Layers (DBL) define a minimal architectural model for
**governed execution of non-deterministic systems**.

The core claim is simple and strict:

> **Normative outcomes must be deterministic, explicit, and replayable,
> even if execution itself is non-deterministic.**

DBL achieves this by:
- expressing normativity *only* as explicit **DECISION** events,
- recording all normative effects in an append-only event stream **V**,
- strictly separating **Governance (G)** from **Boundaries (L)**,
- excluding observational data from decision-making by construction.

The result is a system in which:
- decisions are auditable,
- responsibility is reconstructable,
- replay does not require re-execution,
- and execution outcomes cannot retroactively legitimize decisions.

This work builds on the execution model defined in https://github.com/lukaspfisterch/execution-without-normativity

---

## Conceptual core

DBL is defined by four primitives:

- **V (Behavior)**  
  An append-only, immutable event stream.  
  The sole authoritative record of normativity.

- **DECISION**  
  The only normative event type.  
  Everything else is contextual or observational.

- **G (Governance)**  
  A deterministic function over authoritative inputs.

- **L (Boundaries)**  
  A pre-ontological filter that controls which information may influence G.

Non-deterministic execution (e.g. LLMs, external systems) is treated as
*observational*, never normative.

---

## Structure of the repository

This repository is organized **by claims**, not by implementation.

### Assumptions
Define the scope and threat model under which the claims hold.

- [`assumptions/scope.md`](assumptions/scope.md)
- [`assumptions/threat_model.md`](assumptions/threat_model.md)

### Claims
Each claim is stated independently and precisely.

- [`claims/01_claim_1.md`](claims/01_claim_1.md) - Deterministic Governance  
- [`claims/02_claim_2.md`](claims/02_claim_2.md) - G/L Invariance  
- [`claims/03_claim_3.md`](claims/03_claim_3.md) - Replay Equivalence  
- [`claims/04_claim_4.md`](claims/04_claim_4.md) - Observational Non-Interference  

### Proofs
Each claim has a corresponding proof file.
The main proof files contain the **canonical argument**.
Expanded lemma-based proofs are linked from within those files for reviewers.

- [`proofs/01_deterministic_governance.md`](proofs/01_deterministic_governance.md)
- [`proofs/02_gl_invariance.md`](proofs/02_gl_invariance.md)
- [`proofs/03_replay_equivalence.md`](proofs/03_replay_equivalence.md)
- [`proofs/04_observational_noninterference.md`](proofs/04_observational_noninterference.md)

### Case study
A concrete scenario illustrating the model, not used as proof.

- [`case_study/scenario.md`](case_study/scenario.md)
- [`case_study/traces.md`](case_study/traces.md)

### Paper draft
The LaTeX source assembling the formal model, axioms, claims, proofs, and related work.

- [`paper/main.tex`](paper/main.tex)
- [`paper/refs.bib`](paper/refs.bib)

### Related work
Mapping DBL against existing approaches.

- [`related_work/mapping.md`](related_work/mapping.md)

---

## How to read this work

Recommended reading order:

1. Assumptions  
2. Claims  
3. Proofs  
4. Threat model  
5. Case study  
6. Related work  
7. LaTeX paper

This order mirrors the logical dependency structure of the argument.

---

## Status

- Claim set: **frozen**
- Proof sketches: **complete**
- Expanded proofs: **linked where needed**
- Paper draft: **in progress**

---

## Consistency rule

All guarantees apply **only** under the stated assumptions.

If observational data influences decisions without explicit admission through L,
the model is violated and the claims do not apply.
