# Proof 3 — Replay Equivalence of Normative State

## Theorem (Claim 3)

**Statement:** A replay of the event stream V, starting from initial state $S_0$, produces the same normative state sequence $S_1, S_2, \ldots$ as the original execution, independent of effector outputs, timing, infrastructure, or side observations.

**Formal:** $\text{Replay}(V, S_0) \equiv_{norm} \text{Original}(V, S_0)$

where $\equiv_{norm}$ denotes equality of normative states and DECISION events.

---

## Assumptions Used

- **A1** (Append-only V): V is append-only and events are immutable.
- **A2** (DECISION primacy): All normative effects are represented only by DECISION events.
- **A3** (Authoritative inputs): G consumes only $I_L$; observational data excluded.
- **A4** (Deterministic governance): G deterministically produces DECISION for fixed $I_L$.
- **A5** (Pre-execution decision): DECISION events written before execution.

---

## Proof Structure

We prove replay equivalence through four lemmas:

1. **Lemma 3.1** (V Completeness): V is complete for normative state (no hidden state).
2. **Lemma 3.2** (State Transition Determinism): Normative state transitions are deterministic.
3. **Lemma 3.3** (Observational Irrelevance): Effector outputs do not affect state transitions.
4. **Lemma 3.4** (Ordering Preservation): Event ordering in V is total and preserved.

From these, replay equivalence follows.

---

## Lemma 3.1 (V Completeness for Normativity)

**Statement:** All normative state is derivable exclusively from V. No hidden normative state exists.

**Formal:** $\forall V, S_0: S_{norm} = \mathrm{project\_state}(V, S_0)$.

**Proof:**

By **A2** (DECISION primacy), all normative effects are represented only by DECISION events in V.

Normative state $S$ comprises:
- Current phase (EMPTY, INTENTED, DENIED, ALLOWED, EXECUTED, PROVEN)
- Decision history
- Policy versions applied

All of these are derivable from V:

**Phase:** Determined by event sequence in V
```
project_state(V) → Phase
```
(from dbl-main implementation)

**Decision history:** All DECISION events in V
```
V_norm = {e ∈ V | kind(e) = DECISION}
```

**Policy versions:** Recorded in DECISION event metadata
```
e.data.policy_version
```

No normative state exists outside V:
- No external databases with normative data
- No implicit state in G or L
- No side effects that create normative obligations

By **A1** (Append-only V), V is complete and immutable.

Therefore: V is complete for normative state. **QED**

---

## Lemma 3.2 (State Transition Determinism)

**Statement:** The mapping from current normative state and next event to new normative state is deterministic.

**Formal:** $S_{t+1} = \delta(S_t, e_{t+1})$ where $\delta$ is deterministic.

**Proof:**

Normative state transitions occur only for DECISION events (by A2).

For INTENT events:
- Contextual, not normative
- Update context, not normative state
- Deterministic: same INTENT → same context

For DECISION events:
- Normative effect: ALLOW or DENY
- State transition: $S_t \xrightarrow{\text{DECISION}} S_{t+1}$

The transition function $\delta$ is defined by:
```python
def delta(S, e):
    if e.kind == DECISION:
        if e.data.decision == "ALLOW":
            return State(phase=ALLOWED, ...)
        elif e.data.decision == "DENY":
            return State(phase=DENIED, ...)
    # Other event kinds...
```

This function:
- Has no randomness
- Has no time dependencies
- Has no IO or external state
- Is purely functional

From **dbl-main** contract:
```
"The projection MUST be a pure function:
  - same V bytes -> same State
  - no time, randomness, IO, network, environment, caches, or mutable globals"
```

Therefore: $\delta$ is deterministic. **QED**

---

## Lemma 3.3 (Observational Irrelevance to State Transitions)

**Statement:** Effector outputs and observational data do not affect normative state transitions.

**Formal:** $\delta(S_t, e_{norm}) = \delta(S_t, e_{norm})$ regardless of any $o \in O_{obs}$.

**Proof:**

By **A3** (Authoritative inputs) and proof of Claim 2 (G/L Invariance):
- DECISION events depend only on $I_L$
- $I_L \cap O_{obs} = \emptyset$ (no observational data in authoritative inputs)

By **A2** (DECISION primacy):
- Only DECISION events have normative effect
- EXECUTION events are observational only

The state transition function $\delta$ is defined only over normative events:
```python
def delta(S, e):
    if e.kind == DECISION:
        # Transition based on e.data.decision (ALLOW/DENY)
        # e.data.decision was produced by G(I_L, p_v)
        # No dependency on execution outputs
    elif e.kind == EXECUTION:
        # Observational only, no normative state change
        return S  # State unchanged
```

From **dbl-main** contract:
```
"project_state(V) MUST NOT use:
  - execution trace fields (success, failure_code, exception_type, trace_digest)
  - metadata, caller ids, tenant ids, free-form payloads
  - timestamps, wall-clock time, runtime fields"
```

Therefore: Effector outputs do not affect $\delta$. **QED**

---

## Lemma 3.4 (Ordering Preservation)

**Statement:** Events in V are totally ordered and this ordering is preserved across replay.

**Formal:** $\forall e_i, e_j \in V: t(e_i) < t(e_j) \implies$ replay processes $e_i$ before $e_j$.

**Proof:**

By **A1** (Append-only V), events have unique indices:
```
V = ⟨e_0, e_1, e_2, ...⟩
```

Index $t(e)$ is the position in the sequence.

Replay processes events in order:
```python
S = S_0
for e in V:
    S = delta(S, e)
return S
```

This sequential processing ensures:
- Events processed in index order
- No reordering
- No skipping (unless filtered by event kind)

For normative events specifically:
```python
S = S_0
for e in V:
    if e.kind == DECISION:  # Normative events only
        S = delta(S, e)
```

The order of DECISION events in $V_{norm}$ is preserved.

By **A1**, this order is immutable.

Therefore: Ordering is preserved in replay. **QED**

---

## Main Theorem Proof

**To Prove:** Replaying V produces the same normative state sequence as the original execution.

**Proof:**

**Step 1 (Initial State):**

Both original execution and replay start from the same initial state $S_0$.

**Step 2 (Event Sequence):**

By Lemma 3.1 (V Completeness): All normative events are in V.

Original execution produced: $V = \langle e_0, e_1, e_2, \ldots \rangle$

Replay uses the same V (immutable by A1).

**Step 3 (State Transition):**

For each normative event $e_i \in V$:

Original execution:
$$S_i^{orig} = \delta(S_{i-1}^{orig}, e_i)$$

Replay:
$$S_i^{replay} = \delta(S_{i-1}^{replay}, e_i)$$

By Lemma 3.2 (Determinism): $\delta$ is deterministic.

Therefore: If $S_{i-1}^{orig} = S_{i-1}^{replay}$ and $e_i$ is same, then $S_i^{orig} = S_i^{replay}$.

**Step 4 (Induction):**

Base case: $S_0^{orig} = S_0^{replay}$ (both start from same initial state)

Inductive step: Assume $S_k^{orig} = S_k^{replay}$

For event $e_{k+1}$:
- Same event (V is immutable)
- Same state $S_k$
- Deterministic $\delta$

Therefore: $S_{k+1}^{orig} = \delta(S_k, e_{k+1}) = S_{k+1}^{replay}$

By induction: $\forall i: S_i^{orig} = S_i^{replay}$

**Step 5 (Observational Irrelevance):**

By Lemma 3.3: Effector outputs do not affect $\delta$.

Therefore: Different effector behavior in original vs. replay does not matter.

Original execution may have had:
- Different LLM outputs
- Different timing
- Different infrastructure

But normative state transitions depend only on DECISION events, which are in V.

**Step 6 (Ordering Preservation):**

By Lemma 3.4: Events processed in same order.

Therefore: State sequence is identical.

**Conclusion:**

Replay of V produces:
- Same normative state sequence $S_0, S_1, S_2, \ldots$
- Same DECISION events (from V)
- In same order (from A1)

Independent of:
- Effector outputs (by Lemma 3.3)
- Timing (not in $\delta$)
- Infrastructure (not in $\delta$)
- Observations (excluded by A3)

Therefore: $\text{Replay}(V, S_0) \equiv_{norm} \text{Original}(V, S_0)$ ∎

---

## Failure Modes (If Assumptions Violated)

**If A1 fails (V is mutable):**
- Events can be altered or reordered
- Replay sees different V than original
- Equivalence breaks

**If A2 fails (normativity outside DECISION):**
- Hidden normative state not in V
- Replay cannot reconstruct full normative state
- Equivalence is partial only

**If A3 fails (G accesses observations):**
- DECISION events depend on effector outputs
- Different executions → different observations → different decisions
- Replay with same V but different execution produces different state

**If A4 fails (G is non-deterministic):**
- Same $I_L$ produces different DECISION events
- Replay cannot reproduce decisions
- Equivalence is probabilistic at best

**If A5 fails (decisions after execution):**
- Execution outputs influence decisions
- Replay must re-execute to get same decisions
- But re-execution may differ (non-deterministic effector)

---

## Typical Misunderstandings (from Claim 3)

**"Replay reproduces the world"** — FALSE
- Replay does not re-execute the effector
- Does not reproduce LLM outputs
- Does not reproduce timing or infrastructure state

**"Replay reproduces behavior"** — FALSE  
- Replay does not reproduce execution outputs
- Does not reproduce observational artifacts
- Focuses only on normative state

**"Replay reproduces responsibility"** — TRUE
- Replay reproduces what was decided
- Reproduces policy versions applied
- Reproduces normative obligations

---

## Corollaries

**Corollary 3.1 (Audit from V Alone):**
To audit normative decisions, only V is needed. No access to:
- Original execution environment
- Effector infrastructure
- Timing data
- Observational artifacts

**Corollary 3.2 (Replay Anywhere, Anytime):**
Replay can occur:
- On different infrastructure (cloud vs. on-prem)
- At different times (immediately vs. years later)
- With different effector implementations
- Results are normatively equivalent

**Corollary 3.3 (Compliance Proofs):**
To prove compliance (that correct decisions were made):
1. Provide V
2. Provide policy versions
3. Replay and verify DECISION events

No need to reproduce execution outputs or prove effector correctness.

---

## Implementation Evidence

From **dbl-main** contract (v0.3.0):
```
"Replay(V) ≡_norm Original(V)

Replay begins with the same initial state S0. Events in V are totally ordered 
and immutable. For each normative event e in V, the state transition function 
is deterministic. Therefore each step t produces the same state S_t."
```

From **dbl-core** implementation:
```python
@dataclass(frozen=True)
class BehaviorV:
    events: Tuple[DblEvent, ...]  # Immutable
    
    def digest(self) -> str:
        event_digests = [e.digest() for e in self.events]
        return digest_bytes(json_dumps({"event_digests": event_digests}))
```

Immutability and deterministic digests enable replay verification.

---

## Philosophical Note

Replay is **computation, not narration**.

Narration: Describing what happened (observational).
Computation: Deriving what was decided (normative).

V is a program for normative state reconstruction, not a historical record of execution.

This distinction enables accountability without execution repeatability.

---

## Limitations

This proof assumes:
- V is accessible and uncorrupted
- Initial state $S_0$ is known
- State transition function $\delta$ is correctly implemented

Practical concerns:
- V storage and integrity (blockchain, tamper-evident logs)
- State snapshot mechanisms (checkpointing $S_0$)
- Projection function correctness (testing, verification)

The proof establishes that **under correct implementation**, replay is normatively equivalent.
