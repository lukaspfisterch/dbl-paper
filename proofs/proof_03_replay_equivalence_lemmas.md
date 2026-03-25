# Proof 3 — Replay Equivalence of Normative State

## Proposition (Claim 3)

**Statement:** A replay of the event stream V, starting from the same initial state $S_0$, produces the same sequence of normative states and DECISION events as the original execution.

## Assumptions Used

- **A1** (Append-only V): V is append-only and events are immutable.
- **A2** (DECISION primacy): All normative effects are represented only by DECISION events.
- **A3** (Authoritative inputs): G consumes only $I_L$; observational data is excluded.
- **A4** (Deterministic governance): G deterministically produces DECISION for fixed $I_L$.
- **A5** (Pre-execution decision): DECISION events are written before execution.

## Lemma Chain

1. **Lemma 3.1 (Normative completeness of V).** The normative state is reconstructed from DECISION events in V together with the initial state $S_0$.
2. **Lemma 3.2 (Deterministic state transition).** The replay transition over normative events is deterministic.
3. **Lemma 3.3 (Observational irrelevance).** EXECUTION and PROOF events do not alter normative state.
4. **Lemma 3.4 (Order preservation).** Replay processes the same DECISION events in the same order as the original run.

## Proof Sketch

**Lemma 3.1.** This follows directly from the definition of replay and A2. Only DECISION events contribute normative effect, so the normative projection is determined by the ordered normative subsequence of V and $S_0$.

**Lemma 3.2.** This follows directly from the replay definition and A4. Each recorded DECISION event induces the same normative transition whenever replay is applied to the same event from the same state.

**Lemma 3.3.** This follows directly from A2, A3, and A5. Observational artifacts are excluded from governance and do not create normative effect, so replay ignores them for normative reconstruction.

**Lemma 3.4.** This follows directly from A1. Because V is append-only and immutable, replay sees the same ordered event stream as the original run.

Combining these lemmas, replay from the same initial state processes the same ordered normative events with the same deterministic transitions, yielding the same normative state sequence and DECISION sequence as the original execution. The proposition therefore follows directly from A1--A5.
