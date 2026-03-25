# Proof 2 — Invariance of G/L Separation

## Proposition (Claim 2)

**Statement:** If governance consumes only the authoritative input set $I_L$ admitted by L, then observational data cannot have normative effect without explicit admission, and the separation of G and L is invariant under replay, policy evolution, and effector variation.

## Assumptions Used

- **A1** (Append-only V): V is append-only and events are immutable once written.
- **A2** (DECISION primacy): All normative effects are represented only by explicit DECISION events in V.
- **A3** (Authoritative inputs): Governance consumes only $I_L$ admitted by L; observational data is excluded.
- **A4** (Deterministic governance): For fixed $I_L$ and fixed policy configuration, governance G deterministically produces DECISION events.
- **A6** (Pre-ontological boundaries): L operates before INTENT creation and may reject inputs without creating events in V.

## Lemma Chain

1. **Lemma 2.1 (Domain separation).** Governance operates only on $I_L$, so observational data is outside $\mathrm{dom}(G)$.
2. **Lemma 2.2 (Pre-ontological filtering).** L may reject inputs before any INTENT or DECISION event is created.
3. **Lemma 2.3 (Deterministic governance domain).** For fixed $(I_L, p_v)$, no data outside that pair changes the DECISION produced by G.
4. **Lemma 2.4 (Invariant separation).** Replay, policy evolution, and effector variation do not collapse the distinction between admission by L and decision by G.

## Proof Sketch

**Lemma 2.1.** This follows directly from A3 and the definition of $\mathrm{dom}(G)$. Governance consumes only admitted authoritative inputs, so observational data is excluded from governance.

**Lemma 2.2.** This follows directly from A6 and the definition of INTENT creation. Rejection by L prevents event creation rather than producing a normative decision.

**Lemma 2.3.** This follows directly from A3 and A4. Governance is deterministic over $(I_L, p_v)$, and observational data does not enlarge that domain.

**Lemma 2.4.** Replay preserves the same ordered stream by A1, policy evolution changes policy version rather than the role separation, and effector variation changes observations rather than admitted authoritative inputs. In each case the distinction between L and G remains intact.

Combining these lemmas, observational data has no normative effect unless it is explicitly admitted through L, and the separation between admission and decision remains fixed across the cases named in the claim. The proposition therefore follows directly from A1--A4 and A6.
