# Claim 4 — Observational Non-Interference

## Theorem (Observational Non-Interference)

Observational data O_obs (effector outputs, traces, metrics, timing) cannot directly or indirectly influence normative decisions, except through an explicit, versioned admission mechanism by L and a subsequent decision by G.

Formal non-interference:

G(I_L, O_obs) = G(I_L) for all O_obs, as long as O_obs is not explicitly admitted as authoritative in I_L.

## Assumptions / Preconditions

- **Explicit admission interface:** Only L may admit data into I_L, and each admission is versioned and traceable.
- **No implicit re-ingestion:** Observations are not silently fed back into authoritative inputs, including via metrics pipelines, retries, or heuristics.
- **Deterministic policy evaluation:** G is a function only over I_L, not over runtime artifacts.
- **No side-channel use (scope setting):** Timing, resource behavior, or error codes are decision signals only if explicitly admitted.

## Proof sketch

Decisions arise exclusively from evaluating I_L in G. By definition, O_obs is not in I_L. Since G consumes only I_L, G is invariant with respect to O_obs. Any exception requires explicit admission and is then a model update, not interference.

Therefore observations can explain executions but cannot legitimize decisions.

## Non-goals / Scope limitations

- No claim that observations are useless.
- No claim that policies are optimal.
- No claim of complete side-channel freedom outside the model.

## Failure points

- Retry logic that depends on LLM output.
- Auto-escalation based on specific responses.
- Success-rate as input without explicit admission.
- "If model X returns Y, then reduce max_tokens" without versioned admission.

This claim is defensible only if these patterns are explicitly excluded.
