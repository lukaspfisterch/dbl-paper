# Claim 2 — Invariance of G/L Separation

## Theorem (G/L Invariance)

In a system with an explicit separation between governance G and boundaries L, the normative decision logic is invariant under replay, policy evolution, and effector variation. In particular, no observational signal can have normative effect without explicit, versioned admission through L and G.

Formal:

For all o in O_obs: o not in dom(G) and G = G(I_L), where I_L are the authoritative inputs released by L.

## Assumptions / Preconditions

- **Explicit information release:** L fully defines which data is authoritative and forwarded to G.
- **No implicit channels:** G has no access to effector outputs, timing information, or system state outside I_L.
- **Versioned policy evolution:** Changes in G occur only through explicit policy updates, not through runtime feedback.
- **Replay uses only authoritative inputs:** Observational data is not re-ingested as a decision basis.

## Proof sketch

L defines the interface between the environment and governance. Everything G may see is contained in I_L. Observational outputs O_obs are neither part of I_L nor transitively reachable from I_L. Therefore G cannot be a function of O_obs. Replay reproduces the same mapping G(I_L), independent of observations or effector variation. Normative decisions are invariant with respect to observation.

## Intuition

L decides what may be seen. G decides what is allowed. Observation explains, but does not legitimate.

## Non-goals / Scope limitations

- No protection against a maliciously misconfigured L.
- No claim about policy quality.
- No automatic learning or adaptation of policies.
- No guarantee of complete information isolation; side channels are out of scope.

## Critical failure points

- **Time as an implicit channel:** If G implicitly uses timing, separation is violated.
- **Policy parameter drift:** If policy configuration is fed by observations, the claim fails.
- **Metrics as a Trojan horse:** If metrics (e.g., success rate) enter I_L without governance admission, separation collapses.

This claim is intentionally strict. Separation is treated as an invariant, not a design preference.
