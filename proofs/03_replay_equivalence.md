# Proof — Replay Equivalence of Normative State

## Proposition

A replay of V starting from the same initial state S0 yields the same sequence of normative states and DECISION events as the original execution.

## Assumptions Used

- A1 (Append-only V)
- A2 (DECISION primacy)
- A3 (Authoritative inputs)
- A4 (Deterministic governance)
- A5 (Pre-execution decision)

## Proof Sketch

By A1, V is append-only and immutable, so the replay uses the same ordered events. By A2, normative effects are fully represented by DECISION events in V. By A5, those DECISION events occur before execution and are not influenced by execution outputs. By A4, the DECISION outcomes are deterministic for the recorded inputs and policy configuration. Therefore, replaying V yields the same DECISION sequence and the same normative state sequence as the original execution.

The supplementary lemma chain is provided in [proof_03_replay_equivalence_lemmas.md](./proof_03_replay_equivalence_lemmas.md).
