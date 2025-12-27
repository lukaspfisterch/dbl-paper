# Decision Log

Purpose: record claim-level decisions and changes only.

## Template

- Date: YYYY-MM-DD
  Title: <short>
  Decision: <what was decided>
  Rationale: <1-2 lines>
  Affected files: <paths>

## Entries

- Date: 2025-12-26
  Title: Scope and threat model scaffolding
  Decision: Added scope.md and threat_model.md as required assumption surfaces.
  Rationale: Claims require explicit scope and threat boundaries for review and falsifiability.
  Affected files: assumptions/scope.md, assumptions/threat_model.md

- Date: 2025-12-26
  Title: Accepted core claim set
  Decision: Fixed the four core claims and authored claim files.
  Rationale: Claim set is the normative core for the paper and must remain stable.
  Affected files: claims/01_claim_1.md, claims/02_claim_2.md, claims/03_claim_3.md, claims/04_claim_4.md

- Date: 2025-12-26
  Title: CLAIMS.md marked legacy
  Decision: Treated root CLAIMS.md as legacy in favor of per-claim files.
  Rationale: Canonical claims now live under claims/ for modular review and citation.
  Affected files: CLAIMS.md, claims/01_claim_1.md, claims/02_claim_2.md, claims/03_claim_3.md, claims/04_claim_4.md
