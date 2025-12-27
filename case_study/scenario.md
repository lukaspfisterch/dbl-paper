# Scenario Overview

A user submits a prompt that attempts to influence governance indirectly by framing itself as authoritative guidance. In a non-DBL system, observational signals from downstream execution may influence governance decisions without explicit representation in the decision record. In DBL, governance evaluates only authoritative inputs I_L and records a DECISION before any execution. The scenario contrasts implicit normative drift with explicit decision recording in V.

## Attacker Goal

Cause the system to treat the injected prompt as a governance signal and change the Allow or Deny outcome without explicit admission.

## Typical Failure Without DBL

A non-DBL system may inspect runtime observations and incorporate them into decision logic, effectively allowing execution artifacts to steer policy. This introduces implicit normativity without explicit admission. The resulting normative outcome is not reproducible from the recorded inputs alone.

## DBL Flow

### Authoritative Inputs  
L admits only I_L (authoritative inputs) and excludes observational data. The prompt is represented by a digest and metadata, not raw text.

### Governance Decision  
G evaluates I_L and writes a DECISION event to V before any execution. The decision is produced without reference to observational data.

### Execution (non-normative)  
Execution may occur only after the DECISION. Any resulting observations remain non-normative and do not affect the decision.

## Resulting Normative Outcome

The normative outcome is determined solely by the DECISION event recorded in V. The injected prompt cannot alter the decision unless it changes I_L through explicit admission.

## Why the Outcome Is Replayable

V records the INTENT and DECISION events in order. Given the same I_L and fixed policy configuration, replay yields the same DECISION sequence and normative state.

## Limitations of the Case Study

The scenario is illustrative and does not validate policy quality. It does not address side channels, performance, or external system effects. It only demonstrates explicit separation of normativity from observation.
