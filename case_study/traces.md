# Minimal V Event Outline

Event order is explicit and fixed. The sequence uses digests, not raw content.

1) **INTENT**  
   - prompt_digest  
   - prompt_length  
   - model_id  
   - policy_set_id  
   - tenant_id  
   - channel  

2) **DECISION**  
   - outcome: block  
   - reason_code: content_safety.prompt_injection  

3) *(Optional)* **EXECUTION** — non-normative  
   - labeled observational only  
   - not used for decision or replay  

## Relation to Claims 1–4

This trace records explicit DECISION events before any execution, aligning with determinism under non-deterministic execution (Claim 1). The authoritative inputs are confined to INTENT and exclude observations, preserving G/L separation (Claim 2). Replay over V reproduces the normative outcome because the decision sequence is fully recorded (Claim 3). Observational data, including optional EXECUTION outputs, remains non-normative and does not influence decisions (Claim 4).
