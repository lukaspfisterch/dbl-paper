# Proof — Invariance of G/L Separation

## Theorem

If governance consumes only authoritative inputs I_L admitted by L, and all normativity is expressed exclusively by DECISION events in V, then observational data cannot influence normative outcomes unless explicitly admitted by L.

## Assumptions Used

- A1 (Append-only V)
- A2 (DECISION primacy)
- A3 (Authoritative inputs)
- A4 (Deterministic governance)

## Proof Sketch

By A3, governance consumes only I_L and excludes observational data. By A4, governance decisions are a deterministic function of I_L under fixed policy configuration. By A2, normativity is represented only by explicit DECISION events produced by governance. Therefore, observational data not admitted into I_L cannot influence the production or content of DECISION events. By A1, the resulting DECISION events are immutable in V. Thus, separation between L admission and G decision is invariant with respect to observational data.

## Failure Modes

- If A3 fails, observational inputs can enter governance and violate separation.
- If A4 fails, governance can vary for identical I_L, weakening invariance.
- If A2 fails, normativity can arise outside explicit DECISION events.
- If A1 fails, V can be altered and separation cannot be audited.

## Non-Goals

- No claim about the quality of L admission policies.
- No claim about side-channel resistance beyond explicit admission.
- No claim about multi-tenant or multi-domain composition.

## Extended Proof

A fully expanded, lemma-based proof is provided for reviewer inspection and formal verification. See: [proof_02_gl_invariance_lemmas.md](./proof_02_gl_invariance_lemmas.md)
