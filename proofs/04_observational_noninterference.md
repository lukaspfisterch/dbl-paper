# Proof — Observational Non-Interference

## Theorem

Observational data cannot affect normative decisions as long as governance consumes only I_L and normativity is represented only by DECISION events in V.

## Assumptions Used

- A2 (DECISION primacy)
- A3 (Authoritative inputs)
- A4 (Deterministic governance)

## Proof Sketch

By A3, governance consumes only I_L and excludes observational data. By A4, governance decisions are a deterministic function of I_L and fixed policy configuration. By A2, normative effects are represented only by DECISION events produced by governance. Therefore, observational data cannot influence normative decisions unless it is explicitly admitted into I_L, which would violate A3. Hence observational non-interference holds under the stated assumptions.

## Failure Modes

- If A3 fails, observational data can influence decisions.
- If A4 fails, governance can vary for identical I_L.
- If A2 fails, normativity may arise outside explicit DECISION events.

## Non-Goals

- No claim that observations are useless.
- No claim about policy optimality or fairness.
- No claim about side-channel leakage outside the model.
