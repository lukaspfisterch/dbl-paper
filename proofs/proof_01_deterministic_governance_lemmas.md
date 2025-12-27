# Proof 1 — Deterministic Governance under Non-Deterministic Execution

## Theorem (Claim 1)

**Statement:** Given two boundary runs $R_1$ and $R_2$ with identical authoritative inputs $I_L$, identical boundary configuration $C = (\mathrm{boundary\_version}, \mathrm{boundary\_hash})$, and identical policy versioning $p_v$, the system produces the same ordered sequence of normative DECISION events in $V$, independent of any non-deterministic behavior of the effector.

**Formal:** $(I_L, C, p_v)_1 = (I_L, C, p_v)_2 \implies V_{norm,1} \equiv_{norm} V_{norm,2}$

where $V_{norm} = \{e \in V \mid \text{kind}(e) = \text{DECISION}\}$

---

## Assumptions Used

- **A1** (Append-only V): V is append-only and events are immutable once written.
- **A2** (DECISION primacy): All normative effects are represented only by explicit DECISION events in V.
- **A3** (Authoritative inputs): Governance consumes only $I_L$ admitted by L; observational data is excluded.
- **A4** (Deterministic governance): For fixed $I_L$ and fixed policy configuration, governance G deterministically produces DECISION events.
- **A5** (Pre-execution decision): DECISION events are written before any corresponding execution.

---

## Proof Structure

We prove the theorem through four lemmas:

1. **Lemma 1.1** (I_L Completeness): Authoritative inputs are complete and deterministic.
2. **Lemma 1.2** (G is Pure Function): Governance is a pure, deterministic function over $I_L$.
3. **Lemma 1.3** (Pre-execution Timing): DECISION events are produced before effector execution.
4. **Lemma 1.4** (V Immutability): Once written to V, DECISION events are immutable and ordered.

From these lemmas, the theorem follows directly.

---

## Lemma 1.1 (I_L Completeness)

**Statement:** For fixed boundary configuration $C$, the authoritative input set $I_L$ is complete and deterministically constructed from raw inputs.

**Formal:** $\forall i: L.\text{admit}(i, C) = L.\text{admit}(i, C)$ (deterministic)

and $I_L = \{i \mid L.\text{admit}(i, C) = \text{PASS}\}$ contains all data that may influence governance.

**Proof:**

By definition of $C$ (from formal definitions), $C = (\mathrm{boundary\_version}, \mathrm{boundary\_hash}, L_{\mathrm{rules}})$ where $L_{\mathrm{rules}}$ is a deterministic function.
Here `boundary_hash` denotes the hash of the boundary configuration.

For any raw input $i$ and fixed $C$:
- $L.\text{admit}(i, C)$ evaluates $L_{\text{rules}}$ deterministically
- No access to time, randomness, or IO (by construction of L)
- Therefore: $L.\text{admit}(i, C)$ is a pure function

Since $L_{\mathrm{rules}}$ is versioned (via $\mathrm{boundary\_version}$) and hashed (via $\mathrm{boundary\_hash}$), any change to admission behavior produces a different $C$.

Therefore, for identical $C$ and identical raw input $i$, $L.\text{admit}(i, C)$ produces identical results.

By **A3** (Authoritative inputs), all data influencing governance is in $I_L$, and no observational data $O_{obs}$ is admitted.

Therefore: $I_L$ is complete and deterministically constructed. **QED**

---

## Lemma 1.2 (G is Pure Function)

**Statement:** Governance G is a pure, deterministic function that maps $(I_L, p_v)$ to DECISION events.

**Formal:** $G(I_L, p_v) = G(I_L, p_v)$ for all evaluations (deterministic)

and $\forall o \in O_{obs}: G(I_L, p_v, o) = G(I_L, p_v)$ (observational non-interference)

**Proof:**

By **A4** (Deterministic governance), G is defined as a deterministic function over authoritative inputs.

G has no access to:
- Observational data $O_{obs}$ (by **A3**)
- Effector state or outputs (by construction)
- Time, randomness, or IO (by **A4**)
- Internal mutable state (by **A4**)

Therefore, G is a pure function: same inputs → same outputs.

For any two runs $R_1, R_2$ with:
- $(I_L, p_v)_1 = (I_L, p_v)_2$

We have:
- $G(I_L, p_v)_1 = G(I_L, p_v)_2$

This holds independent of any effector behavior, timing, or observational data.

Therefore: G is pure and deterministic. **QED**

---

## Lemma 1.3 (Pre-execution Timing)

**Statement:** DECISION events are produced and written to V before any effector execution occurs.

**Formal:** $\forall e \in V: \text{kind}(e) = \text{DECISION} \implies t(e) < t(e')$ 

where $e'$ is any EXECUTION event corresponding to the same intent.

**Proof:**

By **A5** (Pre-execution decision), DECISION events are written before any corresponding execution.

The execution flow is:
1. Raw input arrives
2. L admits input → constructs $I_L$
3. INTENT event appended to V (if L passes)
4. G evaluates $(I_L, p_v)$ → produces DECISION
5. DECISION event appended to V
6. **Only then**: If DECISION = ALLOW, effector executes
7. EXECUTION event appended to V (if execution occurred)

Since steps are sequential and V is append-only (**A1**), the ordering is:
- $t(\text{INTENT}) < t(\text{DECISION}) < t(\text{EXECUTION})$

Therefore, DECISION events precede EXECUTION events in V.

This ensures effector outputs cannot influence the DECISION (causal ordering).

Therefore: DECISION events are pre-execution. **QED**

---

## Lemma 1.4 (V Immutability)

**Statement:** Once a DECISION event is written to V, it is immutable and its position in the total order is fixed.

**Formal:** $\forall e \in V_{norm}: e$ is immutable and $t(e)$ is fixed.

**Proof:**

By **A1** (Append-only V), events in V are:
- Immutable once written
- Totally ordered by index $t(e)$
- Never deleted or modified

By **A2** (DECISION primacy), normative effects exist only as DECISION events.

Therefore:
- Once DECISION event $e$ is written at index $t(e)$
- $e$ cannot be changed
- $t(e)$ cannot be changed
- No subsequent events can alter $e$ or its normative effect

The append-only property ensures:
- Replay sees identical DECISION events
- In identical order
- With identical content

Therefore: DECISION events in V are immutable and ordered. **QED**

---

## Main Theorem Proof

**Given:** Two runs $R_1, R_2$ with:
- $(I_L)_1 = (I_L)_2$ (identical authoritative inputs)
- $C_1 = C_2$ (identical boundary configuration)
- $(p_v)_1 = (p_v)_2$ (identical policy version)

**To Prove:** $V_{norm,1} \equiv_{norm} V_{norm,2}$ (normatively equivalent)

**Proof:**

**Step 1:** By Lemma 1.1, identical $C$ and identical raw inputs produce identical $I_L$ deterministically.

Since $(I_L)_1 = (I_L)_2$ is given, this condition holds.

**Step 2:** By Lemma 1.2, G is a pure function over $(I_L, p_v)$.

Therefore:
$$G(I_L, p_v)_1 = G(I_L, p_v)_2$$

This produces identical DECISION events (same outcomes, same reason codes, same policy versions).

**Step 3:** By Lemma 1.3, DECISION events are written before execution.

Therefore, non-deterministic effector behavior in $R_1$ vs $R_2$ cannot affect the DECISION events, as they are causally prior to execution.

**Step 4:** By Lemma 1.4, DECISION events in V are immutable and ordered.

Therefore:
- $V_{norm,1}$ contains the same DECISION events as $V_{norm,2}$
- In the same order (determined by INTENT arrival order, which depends on $I_L$)
- With identical content

**Step 5:** By definition of normative equivalence ($\equiv_{norm}$):

$$V_1 \equiv_{norm} V_2 \iff V_{norm,1} = V_{norm,2} \land \text{order}(V_{norm,1}) = \text{order}(V_{norm,2})$$

From Steps 1-4, both conditions hold.

**Conclusion:**

For identical $(I_L, C, p_v)$, the normative DECISION sequences in $V$ are identical and identically ordered, independent of non-deterministic effector behavior.

Therefore: **Deterministic Governance under Non-Deterministic Execution holds.** ∎

---

## Failure Modes (If Assumptions Violated)

**If A1 fails (V is mutable):**
- DECISION events can be altered post-hoc
- Replay cannot guarantee equivalence
- Determinism is lost

**If A2 fails (normativity outside DECISION):**
- Hidden normative effects bypass V
- Governance cannot be fully replayed
- Equivalence cannot be verified

**If A3 fails (observational inputs reach G):**
- $G(I_L, O_{obs})$ depends on observations
- Different executions → different observations → different decisions
- Determinism is lost

**If A4 fails (G is non-deterministic):**
- Same $(I_L, p_v)$ produces different DECISION events
- Internal randomness, time, or state affect decisions
- Determinism is lost

**If A5 fails (decisions after execution):**
- Effector outputs can influence decisions
- Causality is violated
- Non-deterministic execution affects normative outcomes

---

## Corollaries

**Corollary 1.1 (Replay Determinism):**
Given V from a run, replaying V's normative events produces identical normative state, independent of when/where replay occurs.

**Corollary 1.2 (Audit Independence):**
Auditing decisions requires only $(I_L, C, p_v, V)$. No access to effector infrastructure, timing, or observations needed.

**Corollary 1.3 (Observational Irrelevance):**
For governance purposes, effector outputs, traces, timing, and errors are strictly non-normative. They explain but do not legitimate.

---

## Implementation Evidence

From **dbl-policy** contract (v0.1.0):
```
"Given the same PolicyContext, a policy MUST return the same PolicyDecision.
 No time, randomness, IO, network, environment variables, or mutable globals."
```

From **dbl-core** contract (v0.3.0):
```
"V is append-only. Events are immutable once written."
```

From **GL_SEPARATION** contract (v1.0):
```
"Deterministic constraint enforcement that does not introduce normative 
 outcomes MAY operate without emitting events."
```

These contracts enforce A1-A5 at the implementation level.

---

## Limitations

This proof assumes:
- L and G are correctly implemented (no bugs)
- No Byzantine failures (adversarial component modification)
- No side channels (timing attacks, resource consumption)

These are system-level concerns outside the model's scope.

The proof establishes that **under the stated assumptions**, deterministic governance is guaranteed by the architectural separation and contract enforcement.
