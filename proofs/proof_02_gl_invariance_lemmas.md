# Proof 2 — Invariance of G/L Separation

## Theorem (Claim 2)

**Statement:** If governance consumes only the authoritative input set $I_L$ admitted by L, then observational data cannot have normative effect without explicit admission, and the separation of G and L is invariant under replay, policy evolution, and effector variation.

**Formal:** $\text{dom}(G) = I_L \times P_v$, $\text{dom}(L) = \text{Input} \times C$, and $I_L \cap O_{obs} = \emptyset$

$\implies \forall O_{obs}, \forall$ effector variation $E$: $G(I_L, p_v) = G(I_L, p_v)$ (invariant)

---

## Assumptions Used

- **A1** (Append-only V): V is append-only and events are immutable once written.
- **A2** (DECISION primacy): All normative effects are represented only by explicit DECISION events in V.
- **A3** (Authoritative inputs): Governance consumes only $I_L$ admitted by L; observational data is excluded.
- **A4** (Deterministic governance): For fixed $I_L$ and fixed policy configuration, governance G deterministically produces DECISION events.
- **A6** (Pre-ontological boundaries): L operates pre-ontologically and may reject inputs before any INTENT is created.

---

## Proof Structure

We prove the theorem through four lemmas:

1. **Lemma 2.1** (Domain Disjointness): G and L operate on disjoint input domains with respect to observational data.
2. **Lemma 2.2** (L Pre-ontological Filtering): L enforces constraints without producing normativity.
3. **Lemma 2.3** (G Domain Closure): Governance is closed over $I_L$; no external data enters $\text{dom}(G)$.
4. **Lemma 2.4** (Separation Invariance): The G/L separation is invariant under replay, policy evolution, and effector variation.

From these lemmas, the theorem follows directly.

---

## Lemma 2.1 (Domain Disjointness)

**Statement:** The input domain of G and the observational domain $O_{obs}$ are disjoint. L mediates all input to G, and only authoritative inputs pass the boundary.

**Formal:** $\text{dom}(G) \cap O_{obs} = \emptyset$

**Proof:**

By **A3** (Authoritative inputs), governance consumes only $I_L$, the set of inputs explicitly admitted by L.

$I_L$ is defined as: $I_L = \{i \mid L.\text{admit}(i, C) = \text{PASS}\}$

Observational data $O_{obs}$ includes execution outputs, traces, timing, metrics, and effector state. By construction of L, these are not in the raw input domain of L.admit.

Therefore:
- $O_{obs} \notin \text{dom}(L.\text{admit})$ (L does not process observations)
- $O_{obs} \notin I_L$ (observations cannot be admitted)
- $O_{obs} \notin \text{dom}(G)$ (G consumes only $I_L$)

Hence: $\text{dom}(G) \cap O_{obs} = \emptyset$. **QED**

---

## Lemma 2.2 (L Pre-ontological Filtering)

**Statement:** When L rejects an input, no normative event is created. L enforces constraints without producing normativity.

**Formal:** $L.\text{admit}(i, C) = \text{REJECT} \implies \nexists e \in V: \text{kind}(e) \in \{\text{INTENT}, \text{DECISION}\}$ for input $i$.

**Proof:**

By **A6** (Pre-ontological boundaries), L operates before the ontological boundary — before any event is written to V.

If $L.\text{admit}(i, C) = \text{REJECT}$:
- No INTENT event is created (the input never enters the event stream)
- G is never invoked (no $I_L$ is constructed for this input)
- No DECISION event is produced

By **A2** (DECISION primacy), all normativity is in DECISION events. Since no DECISION is produced, no normative effect occurs.

L's rejection is a constraint, not a decision. It shapes what enters $\text{dom}(G)$ without itself producing normative content.

Therefore: L filters pre-ontologically without creating normativity. **QED**

---

## Lemma 2.3 (G Domain Closure)

**Statement:** Governance G is closed over $(I_L, p_v)$. No data outside this pair can influence G's output.

**Formal:** $\forall x \notin (I_L \times P_v): G(I_L, p_v, x) = G(I_L, p_v)$

**Proof:**

By **A4** (Deterministic governance), G is a pure function over $(I_L, p_v)$:
- No access to $O_{obs}$ (by **A3** and Lemma 2.1)
- No access to effector state or outputs (by construction)
- No access to time, randomness, or IO (by **A4**)
- No access to L's internal state or rejection history (by component ownership)

G's signature is: $G: I_L \times P_v \to \text{DECISION}$

Any additional argument $x$ is provably excluded from G's evaluation:
- If $x \in O_{obs}$: excluded by A3
- If $x$ is effector state: excluded by A4
- If $x$ is L internal state: excluded by component ownership

Therefore: G is closed over $(I_L, p_v)$. **QED**

---

## Lemma 2.4 (Separation Invariance)

**Statement:** The separation between G and L is invariant under replay, policy evolution, and effector variation.

**Formal:** For any replay $R'$, policy update $p_v \to p_{v'}$, or effector variation $E \to E'$:

$\text{dom}(G) \cap O_{obs} = \emptyset$ continues to hold.

**Proof:**

**Under replay:**
Replay processes the same V (by **A1**, immutable). By Lemma 2.3, G is closed over $(I_L, p_v)$, both of which are recorded in V. Replay does not introduce new observational data into $\text{dom}(G)$. The separation holds.

**Under policy evolution ($p_v \to p_{v'}$):**
Policy evolution changes $p_v$ but does not change the domain structure. G still consumes $(I_L, p_{v'})$ — the input domain remains $I_L \times P_v$. By **A3**, $O_{obs}$ remains excluded. The separation holds.

**Under effector variation ($E \to E'$):**
Effector variation changes execution behavior and therefore $O_{obs}$. But by Lemma 2.1, $O_{obs} \notin \text{dom}(G)$. By Lemma 2.3, G is closed and cannot observe effector changes. The separation holds.

Therefore: G/L separation is invariant under all three conditions. **QED**

---

## Main Theorem Proof

**Given:**
- G consumes only $I_L$ admitted by L (A3)
- All normativity is in DECISION events (A2)
- L operates pre-ontologically (A6)

**To Prove:** Observational data cannot have normative effect without explicit admission, and the separation is invariant.

**Proof:**

**Step 1:** By Lemma 2.1, $\text{dom}(G) \cap O_{obs} = \emptyset$. Observational data is structurally excluded from governance.

**Step 2:** By Lemma 2.2, L operates pre-ontologically. Rejection creates no normativity. Only admitted inputs reach G.

**Step 3:** By Lemma 2.3, G is closed over $(I_L, p_v)$. No external data can influence governance output.

**Step 4:** By Lemma 2.4, this separation is invariant under replay, policy evolution, and effector variation.

**Conclusion:**

Observational data cannot influence normative outcomes unless explicitly admitted as authoritative input through a versioned boundary update (which changes $C$ and therefore changes the model). The separation between G (governance) and L (boundaries) is maintained invariantly.

Therefore: **Invariance of G/L Separation holds.** $\blacksquare$

---

## Failure Modes (If Assumptions Violated)

**If A3 fails (observational inputs reach G):**
- $O_{obs}$ enters $\text{dom}(G)$
- Domain disjointness is violated
- Different executions produce different observations, leading to different decisions

**If A6 fails (L produces normativity):**
- L rejection becomes a normative act
- Separation between filtering and deciding is lost
- Normativity leaks outside DECISION events

**If A4 fails (G is non-deterministic):**
- G may consume hidden state or IO
- Domain closure is violated
- Separation cannot be verified

**If A1 fails (V is mutable):**
- Replay may see different events
- Invariance under replay cannot be established

---

## Corollaries

**Corollary 2.1 (Boundary Update Transparency):**
If $O_{obs}$ must influence future governance, it requires an explicit boundary update: $C \to C'$ with new $L_{\text{rules}}$. This update is versioned and auditable.

**Corollary 2.2 (L Independence from G):**
L's admission decisions do not depend on G's prior decisions or outputs. L and G are compositionally independent.

**Corollary 2.3 (Observational Enrichment without Normative Effect):**
Observational data may be recorded in V (as EXECUTION or PROOF events) for diagnostic or enrichment purposes without affecting any normative property of the system.
