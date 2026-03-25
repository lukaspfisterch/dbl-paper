# Proof 4 — Observational Non-Interference

## Proposition (Claim 4)

**Statement:** Observational data cannot affect normative decisions when DECISION is produced before execution and governance consumes only $I_L$, with normativity represented only by DECISION events in V.

## Assumptions Used

- **A2** (DECISION primacy): All normative effects are represented only by explicit DECISION events in V.
- **A3** (Authoritative inputs): G consumes only $I_L$; observational data is excluded.
- **A4** (Deterministic governance): G deterministically produces DECISION for fixed $I_L$.
- **A5** (Pre-execution decision): DECISION events are written before execution.

## Lemma Chain

1. **Lemma 4.1 (Governance domain restriction).** Observational data is outside the domain of governance.
2. **Lemma 4.2 (Deterministic decision rule).** For fixed $(I_L, p_v)$, governance produces the same DECISION event.
3. **Lemma 4.3 (Causal ordering).** DECISION events precede EXECUTION events within a run.
4. **Lemma 4.4 (Normative exclusivity).** Only DECISION events carry normative effect.

## Proof Sketch

**Lemma 4.1.** This follows directly from A3 and the definition of $I_L$. Observational data is excluded from the inputs that governance may consume.

**Lemma 4.2.** This follows directly from A4. Once the authoritative input set and policy version are fixed, the decision is fixed as well.

**Lemma 4.3.** This follows directly from A5. Execution occurs only after the DECISION has already been written, so observational outputs cannot feed back into that decision within the same run.

**Lemma 4.4.** This follows directly from A2. Since normativity is represented only by DECISION events, observational artifacts cannot independently create normative effect.

Combining these lemmas, observational variation does not enter governance, does not alter the deterministic decision rule, and occurs only after the normative event has already been recorded. The proposition therefore follows directly from A2--A5.
