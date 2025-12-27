# Proof — Replay Equivalence of Normative State

## Theorem

A replay of V from the same initial state yields the same sequence of normative states and DECISION events as the original execution.

## Assumptions Used

- A1 (Append-only V)
- A2 (DECISION primacy)
- A4 (Deterministic governance)
- A5 (Pre-execution decision)

## Proof Sketch

By A1, V is append-only and immutable, so the replay uses the same ordered events. By A2, normative effects are fully represented by DECISION events in V. By A5, those DECISION events occur before execution and are not influenced by execution outputs. By A4, the DECISION outcomes are deterministic for the recorded inputs and policy configuration. Therefore, replaying V yields the same DECISION sequence and the same normative state sequence as the original execution.

## Failure Modes

- If A1 fails, replay can diverge due to mutation or reordering of V.
- If A2 fails, normativity may be missing from V and cannot be replayed.
- If A5 fails, execution can influence decisions and replay equivalence breaks.
- If A4 fails, governance outcomes may differ across replays.

## Non-Goals

- No claim about replay of execution outputs.
- No claim about reproducing external side effects.
- No claim about completeness of observations.
