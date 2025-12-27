# Claim 3 — Replay Equivalence of Normative State

## Theorem (Replay Equivalence)

A replay of the event stream V, starting from the initial state S0, produces the same normative state sequence S1, S2, ... as the original execution, independent of effector outputs, timing, infrastructure, or side observations.

Formal:

Replay(V) ≡_norm Original(V)

where ≡_norm denotes equality of normative states and DECISION events.

## Assumptions / Preconditions

- **V is complete for normativity:** Every normative state is derivable exclusively from V.
- **DECISION events are sufficient:** All normative effects are explicitly represented as DECISION events in V.
- **No hidden normative state:** No normative state exists outside what can be reconstructed from V.
- **Deterministic state transition:** The mapping (S_t, e_{t+1}) -> S_{t+1} is deterministic for all normative event types.

## Proof sketch

Replay begins with the same initial state S0. Events in V are totally ordered and immutable. For each normative event e in V, the state transition function is deterministic. Therefore each step t produces the same state S_t. Effector outputs are not part of V_norm and have no transition effect. The normative state sequence is replay-invariant.

## Intuition

If everything that counts is written down, replay is computation, not narration.

## Non-goals / Scope limitations

- No guarantee of equality for effector outputs.
- No claim about external side effects.
- No claim about completeness of observations.
- No claim about semantic correctness of decisions.

The claim concerns normativity only, not physical outcomes.

## Typical misunderstandings

- "Replay reproduces the world" — false.
- "Replay reproduces behavior" — false.
- "Replay reproduces responsibility" — true.

## Failure points

- Policies have implicit state.
- DECISION events are incomplete.
- Ordering or versioning is ambiguous.
- Normativity is produced outside V.

These failure points are exactly what the model forbids.
