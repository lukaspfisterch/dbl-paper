# Proof 4 — Observational Non-Interference

## Theorem (Claim 4)

**Statement:** Observational data $O_{obs}$ (effector outputs, traces, metrics, timing) cannot directly or indirectly influence normative decisions, except through an explicit, versioned admission mechanism by L and a subsequent decision by G.

**Formal:** $G(I_L, O_{obs}) = G(I_L)$ for all $O_{obs}$

as long as $O_{obs}$ is not explicitly admitted as authoritative in $I_L$.

In other words: **Observations can explain executions but cannot legitimize decisions.**

---

## Assumptions Used

- **A1** (Append-only V): V is append-only and events are immutable.
- **A2** (DECISION primacy): All normative effects are represented only by explicit DECISION events in V.
- **A3** (Authoritative inputs): G consumes only $I_L$; observational data is excluded.
- **A4** (Deterministic governance): G deterministically produces DECISION for fixed $I_L$.
- **A5** (Pre-execution decision): DECISION events written before execution.

---

## Proof Structure

We prove observational non-interference through four lemmas:

1. **Lemma 4.1** (G Cannot Access $O_{obs}$): By construction, G has no access to observational data.
2. **Lemma 4.2** (L Filters $O_{obs}$): L rejects observational data from entering $I_L$.
3. **Lemma 4.3** (Causal Ordering): DECISION precedes EXECUTION, preventing feedback loops.
4. **Lemma 4.4** (Explicit Admission Traceability): Any path from $O_{obs}$ to $\text{dom}(G)$ is versioned and auditable.

From these, observational non-interference follows.

---

## Lemma 4.1 (G Cannot Access Observational Data)

**Statement:** By construction, G has no access to observational data $O_{obs}$.

**Formal:** $G$ is defined only over $(I_L, p_v)$ and has no inputs or channels to observe $O_{obs}$.

**Proof:**

G is defined as:
$$G: (I_L, p_v) \to \text{DECISION}$$

By **A3** (Authoritative inputs), G consumes only $I_L$ and does not admit $O_{obs}$ as input.

$O_{obs}$ is defined as:
$$O_{obs} = \{\text{traces, outputs, timing, errors, metrics, runtime artifacts}\}$$

By construction of G (from **dbl-policy** contract):
- G is a pure function over $I_L$
- G has no access to:
  - Execution traces or outputs
  - Timing information
  - Error details beyond reason codes
  - Metrics or telemetry
  - Any runtime artifacts

From **dbl-policy** implementation:
```python
class Policy(Protocol):
    def evaluate(self, context: PolicyContext) -> PolicyDecision:
        # Pure function: (I_L, p_v) → DECISION
        # No access to:
        # - time.time(), datetime.now()
        # - random.random()
        # - file I/O, network, database
        # - execution traces or outputs
        # - global mutable state
```

PolicyContext validation enforces:
```python
_FORBIDDEN_INPUT_KEYS = {
    "trace", "trace_digest", "execution", "execution_trace",
    "success", "failure_code", "exception_type", "exception",
    "runtime", "runtime_ms", "output", "error", "result"
}

def __post_init__(self):
    for key in self.inputs.keys():
        if key in _FORBIDDEN_INPUT_KEYS:
            raise ValueError(f"Observational key not allowed: {key}")
```

Therefore: G has no mechanism to access $O_{obs}$. **QED**

---

## Lemma 4.2 (L Filters Observational Data)

**Statement:** L rejects all observational data from entering $I_L$.

**Formal:** $\forall o \in O_{obs}: L.\text{admit}(o, C) = \text{REJECT}$

Therefore: $I_L \cap O_{obs} = \emptyset$

**Proof:**

This follows from Claim 2 (G/L Invariance): L prevents observational data from entering $I_L$ under fixed boundary configuration $C$.

Implementation evidence (non-normative):

By construction of L (from GL_SEPARATION contract):
```
"L controls information flow before, during, and after decisions.
 L defines and enforces:
  - allowed context schema
  - non-observability rules
  - input sanitization and shaping
  - output filtering"
```

L's admission rules explicitly check for observational data:
```python
def is_observational(data: Any) -> bool:
    """Check if data is observational (execution-dependent)."""
    observational_markers = [
        "trace", "execution", "output", "result", 
        "error", "exception", "runtime", "timing",
        "success", "failure", "metric"
    ]
    
    if isinstance(data, dict):
        for key in data.keys():
            if any(marker in key.lower() for marker in observational_markers):
                return True
    
    return False

def admit(input: Any, config: C) -> AdmitResult:
    if is_observational(input):
        return REJECT
    # ... other checks
    return PASS
```

From GL_SEPARATION contract:
```
"L MUST ensure:
  - G cannot directly consume observational data.
  - Only authoritative inputs reach normative decision logic."
```

Therefore: $O_{obs}$ cannot enter $I_L$ without explicit rule change. **QED**

---

## Lemma 4.3 (Causal Ordering Prevents Feedback)

**Statement:** DECISION events precede EXECUTION events, preventing observational feedback loops.

**Formal:** $\forall e_{dec}, e_{exec} \in V$: 

If $e_{dec}$.kind = DECISION and $e_{exec}$.kind = EXECUTION and they correspond to the same intent,

then $t(e_{dec}) < t(e_{exec})$ (decision happens before execution).

**Proof:**

By **A5** (Pre-execution decision): DECISION events are written before execution.

The execution flow is strictly ordered:
```
1. Raw input arrives
2. L.admit(input, C) → PASS or REJECT
3. If PASS: INTENT appended to V
4. G.evaluate(I_L, p_v) → DECISION
5. DECISION appended to V
6. ← DECISION is now in V
7. If DECISION = ALLOW: Execute effector
8. EXECUTION appended to V
```

By **A1** (Append-only V), events are ordered by append time:
- $t(\text{INTENT}) < t(\text{DECISION}) < t(\text{EXECUTION})$

This causal ordering ensures:
- Effector outputs (in EXECUTION) cannot influence DECISION
- No temporal feedback loop
- No retry-based decision adjustment

From **dbl-boundary-service** flow:
```python
# Step 1: Produce DECISION
intent = create_intent(request)
V.append(intent)

decision = policy.evaluate(context)  # ← No execution yet
V.append(decision_event)

# Step 2: Only if ALLOW, then execute
if decision.outcome == ALLOW and not dry_run:
    execution = kernel.execute(psi)
    V.append(execution_event)  # ← After DECISION
```

Therefore: Causal ordering prevents observational feedback. **QED**

---

## Lemma 4.4 (Explicit Admission Traceability)

**Statement:** Any path that allows $O_{obs}$ to influence G requires explicit, versioned admission and is auditable.

**Formal:** If $o \in O_{obs}$ influences G, then:
1. $\exists$ explicit admission rule change in L
2. boundary_version incremented
3. Change is recorded in V metadata
4. Audit trail exists

**Proof:**

Suppose $o \in O_{obs}$ needs to influence a decision (e.g., "use success rate to adjust policy").

**Path Analysis:**

**Implicit Path (Forbidden):**
```
o ∈ O_obs → [hidden channel] → G(I_L, o)
```
This is prevented by:
- Lemma 4.1: G cannot access $o$
- Lemma 4.2: L rejects $o$
- No covert channel exists (by construction)

**Explicit Path (Only Allowed):**
```
1. Define new L rule: "admit success_rate as authoritative"
2. Update boundary configuration C' = (..., boundary_version + 1, ...)
3. L.admit(success_rate, C') = PASS
4. success_rate ∈ I_L'
5. G(I_L', p_v) uses success_rate
6. DECISION influenced by success_rate
```

This explicit path requires:
- L rule change (versioned in C)
- boundary_version increment (traceable)
- Recorded in INTENT metadata:
  ```json
  {
    "event_kind": "INTENT",
    "data": {...},
    "metadata": {
      "boundary_version": "1.1",  # ← Incremented
      "boundary_config_hash": "sha256:abc...",
      "admitted_fields": ["prompt", "success_rate"]  # ← Explicit
    }
  }
  ```

From GL_SEPARATION contract:
```
"boundary_version SHOULD be recorded as authoritative metadata 
 on admitted INTENT events."
```

Therefore: Any admission of $O_{obs}$ is:
- Explicit (requires L rule change)
- Versioned (boundary_version tracks)
- Auditable (metadata in V)

**QED**

---

## Main Theorem Proof

**To Prove:** Observational data cannot influence decisions except through explicit, versioned admission.

**Formal:** $G(I_L, O_{obs}) = G(I_L)$ for all $O_{obs}$ not explicitly admitted.

**Proof:**

**Step 1 (G's Domain Restriction):**

By Lemma 4.1: $O_{obs} \notin \text{dom}(G)$

G is defined as: $G: (I_L, p_v) \to \text{DECISION}$

G has no parameters for $O_{obs}$, no access mechanism, no API.

**Step 2 (L's Filtering):**

By Lemma 4.2: $I_L \cap O_{obs} = \emptyset$

Even if external system tries to pass observational data, L rejects it.

**Step 3 (Causal Ordering):**

By Lemma 4.3: $t(\text{DECISION}) < t(\text{EXECUTION})$

Effector outputs are produced AFTER decision, cannot causally influence it.

**Step 4 (Mathematical Consequence):**

Since:
- G consumes only $I_L$ (Lemma 4.1)
- $I_L \cap O_{obs} = \emptyset$ (Lemma 4.2)
- $O_{obs}$ is produced after DECISION (Lemma 4.3)

Since $O_{obs}$ is neither admitted into $I_L$ nor available before DECISION, there exists no causal or dataflow path from $O_{obs}$ to $G$ under the model.

Therefore:
$$G(I_L, O_{obs}) = G(I_L)$$

because G has no way to access, observe, or be influenced by $O_{obs}$.

**Step 5 (Explicit Admission Exception):**

If $O_{obs}$ is to influence decisions, by Lemma 4.4:
- Explicit admission required (L rule change)
- Versioned (boundary_version increments)
- Auditable (metadata recorded)
- Then $o \in I_L'$ (new authoritative set)
- Then $G(I_L', p_v)$ legitimately uses $o$

This is **not interference** — it is **explicit model update**.

**Conclusion:**

Observational data has no normative effect without explicit admission.

Observations can:
- Explain what happened (telemetry, traces)
- Debug execution (error logs)
- Optimize performance (metrics)

But cannot:
- Change what was decided (DECISION is immutable)
- Influence future decisions (without explicit admission)
- Create normative obligations (only DECISION events do)

Therefore: **Observational Non-Interference holds.** ∎

---

## Failure Modes (If Assumptions Violated)

**If A3 fails (G accesses $O_{obs}$):**
- Direct interference
- Example: `if execution_trace.success: decision = ALLOW`
- Violation: Execution output influences decision

**If A4 fails (G is non-deterministic):**
- $G(I_L)$ varies on re-evaluation
- Cannot verify interference (noise vs. signal)

**If A5 fails (decision after execution):**
- Execution outputs available before decision
- Temporal feedback possible
- Example: "Execute speculatively, then decide based on output"

**If A6 fails (L does not enforce boundaries):**
- Observational data leaks into $I_L$
- Example: Metrics pipeline feeds back as authoritative input

---

## Critical Failure Points (from Claim 4)

**Retry Logic:**
```python
# FORBIDDEN:
for attempt in range(3):
    result = llm_call(prompt)
    if is_toxic(result):  # ← Observational!
        adjust_policy()  # ← Normative influence!
        continue
```

This violates non-interference because:
- `is_toxic(result)` is observational (depends on execution)
- `adjust_policy()` is normative (changes decisions)
- Feedback loop created

**Correct Approach:**
```python
# CORRECT:
decision = policy.evaluate(I_L)  # ← No execution yet
V.append(decision_event)

if decision.outcome == ALLOW:
    result = llm_call(prompt)
    V.append(execution_event)  # ← Observational only
```

**Auto-Escalation:**
```python
# FORBIDDEN:
if response_contains("refusal"):  # ← Observational!
    escalate_to_human()  # ← Normative effect!
```

**Correct Approach:**
- If "refusal detection" is needed, make it authoritative:
  - Define L rule: "admit refusal_classifier output"
  - Version: boundary_version++
  - Then use in G

**Success Rate Adjustment:**
```python
# FORBIDDEN:
success_rate = count_successes() / total  # ← Observational metric!
if success_rate < 0.9:
    relax_policy()  # ← Normative influence!
```

**Correct Approach:**
- Success rate is observational
- To use in governance:
  - Explicit admission via L
  - Versioned (boundary_version++)
  - Recorded in metadata

---

## Corollaries

**Corollary 4.1 (Observations Explain, Not Legitimate):**
Execution traces and outputs explain system behavior but cannot create normative obligations or change decisions.

**Corollary 4.2 (Deterministic Decisions):**
Since decisions cannot depend on observational data, they are deterministic for fixed $(I_L, p_v)$.

**Corollary 4.3 (Audit Without Re-execution):**
To audit decisions, no need to re-execute effector. Only need $(V, I_L, p_v)$.

**Corollary 4.4 (Prohibition of Adaptive Policies):**
Policies that learn from execution outputs violate non-interference unless:
- Learning is offline (not during decision-making)
- Updated policies are versioned
- Observations are explicitly admitted

---

## Implementation Evidence

From **dbl-policy** contract (v0.1.0):
```
"A policy MUST NOT:
  - Execute tasks, make LLM calls, or perform I/O
  - Call kl-kernel-logic or any execution layer
  - Read execution traces, success/failure codes, or runtime artifacts
  - Depend on EXECUTION or PROOF events from V"
```

From **GL_SEPARATION** contract (v1.0):
```
"Observational data:
  - Telemetry, traces, debug output, logs, runtime measurements.
  - These MUST NOT become normative.

Principle:
> If data depends on non-deterministic execution behavior, 
  it is observational and non-normative."
```

From **dbl-core** implementation:
```python
class DblEvent:
    DETERMINISTIC_FIELDS = ("event_kind", "correlation_id", "data")
    OBSERVATIONAL_FIELDS = ("observational",)
    
    def digest(self):
        # Uses ONLY deterministic fields
        # Observational fields excluded
```

These contracts and implementations enforce observational non-interference.

---

## Philosophical Note

The distinction between **explanation** and **legitimation** is central:

**Explanation (Observational):**
- "The LLM produced toxic output because the prompt contained trigger words."
- Describes causal mechanism
- Helps understand what happened
- Non-normative

**Legitimation (Normative):**
- "The LLM call was allowed because policy P version V evaluated authoritative input I and decided ALLOW."
- Justifies decision
- Establishes accountability
- Normative

DBL ensures:
- Explanations are rich (full traces, telemetry)
- Legitimations are deterministic (only from $I_L$)
- No conflation between the two

This mirrors legal reasoning: evidence explains, but law legitimates.

---

## Limitations

This proof assumes:
- No covert channels (timing, resource usage) beyond explicit model
- L and G correctly implemented (no bugs)
- No adversarial component modification

Practical side channels:
- Timing attacks (observing execution time)
- Resource monitoring (CPU, memory patterns)
- Error code side channels

These are **out of scope** for the formal model but should be considered in implementation.

The proof establishes that **within the architectural model**, observational non-interference is guaranteed.

---

## Extensions

**Multi-round Interactions:**
If system has multiple rounds (e.g., conversation), each round produces new INTENT:
```
Round 1: INTENT₁ → DECISION₁ → EXECUTION₁
Round 2: INTENT₂ → DECISION₂ → EXECUTION₂
```

Non-interference holds **per-round**:
- DECISION₁ independent of EXECUTION₁ outputs
- DECISION₂ independent of EXECUTION₂ outputs

But INTENT₂ may legitimately include conversation history (authoritative).

**Federated Governance:**
If multiple governance instances coordinate:
- Each G instance has own $(I_L, p_v)$
- Coordination via explicit message passing
- Messages are authoritative (not observational)
- Non-interference holds per-instance

These extensions preserve non-interference while enabling richer workflows.
