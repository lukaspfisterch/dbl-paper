# Proof 1 — Deterministic Governance under Non-Deterministic Execution

## Proposition (Claim 1)

**Statement:** For fixed authoritative inputs $I_L$, fixed boundary configuration $C$, and fixed policy version $p_v$, the ordered sequence of DECISION events written to $V$ is deterministic and unaffected by non-deterministic execution.

## Assumptions Used

- **A1** (Append-only V): V is append-only and events are immutable once written.
- **A2** (DECISION primacy): All normative effects are represented only by explicit DECISION events in V.
- **A3** (Authoritative inputs): Governance consumes only $I_L$ admitted by L; observational data is excluded.
- **A4** (Deterministic governance): For fixed $I_L$ and fixed policy configuration, governance G deterministically produces DECISION events.
- **A5** (Pre-execution decision): DECISION events are written before any corresponding execution.

## Lemma Chain

1. **Lemma 1.1 (Deterministic admission).** For fixed $C$, the authoritative input set $I_L$ is determined by L and excludes observational data.
2. **Lemma 1.2 (Deterministic governance).** For fixed $(I_L, p_v)$, governance produces the same DECISION event.
3. **Lemma 1.3 (Pre-execution ordering).** DECISION events are written to $V$ before any corresponding EXECUTION event.
4. **Lemma 1.4 (Immutable normative record).** Once written, DECISION events remain fixed in content and order within $V$.

## Proof Sketch

**Lemma 1.1.** This follows directly from the definitions of $C$ and $I_L$ together with A3. L determines which inputs are authoritative, and observational data is excluded from that set.

**Lemma 1.2.** This follows directly from A4. Governance is deterministic for fixed authoritative inputs and fixed policy version.

**Lemma 1.3.** This follows directly from A5. Any DECISION event is recorded before the corresponding execution begins, so execution behavior is causally downstream of the decision.

**Lemma 1.4.** This follows directly from A1 and A2. Since V is append-only and only DECISION events are normative, the normative record is fixed once written.

Combining these lemmas, identical $(I_L, C, p_v)$ yields the same ordered DECISION sequence in $V$, and non-deterministic execution cannot alter that sequence. The proposition therefore follows directly from A1--A5.
