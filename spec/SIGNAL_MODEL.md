# Decision Physics
## Signal Model Specification
Version: 0.1 (Draft)

---

## 1. Status

This document defines the normative structure and operational constraints for Signals within the Decision Physics framework.

This specification is intended for public review and implementation.

---

## 2. Abstract

Signals are deterministic, recomputable metrics derived from curated evidence associated with IT entities.

Signals:

- Are derived from evidence and entity metadata
- Are policy-defined and versioned
- May be ad hoc or composite
- Are non-authoritative
- Must be reproducible

Signals do not replace evidence.  
Evidence remains canonical.

---

## 3. Scope

This specification applies to signals derived from:

- Base Entity Envelope metadata
- Transaction Log entries
- Evidence domain artifacts (security, deployment, testing, documentation, technology)
- Organizational policy definitions

Signals may combine inputs across domains.

---

## 4. Terminology

**Primitive Signal**  
A signal computed directly from a single domain of evidence (e.g., documentation_age, deployment_drift).

**Composite Signal**  
A signal computed from one or more primitive signals and/or cross-domain evidence (e.g., documentation_health, operational_pressure).

**Policy Version**  
A versioned definition of thresholds, weights, or classification rules used in signal computation.

**Signal Definition**  
A formal declaration of how a signal is computed, including required inputs and policy references.

---

## 5. Signal Properties

All signals MUST adhere to the following properties:

1. Deterministic  
2. Recomputable  
3. Versioned  
4. Traceable to input evidence  
5. Non-authoritative  

Signals MUST NOT mutate or overwrite evidence artifacts.

---

## 6. Signal Structure

Each computed signal record MUST include:

- schema_name
- schema_version
- entity_id
- signal_name
- signal_version
- policy_version
- input_domains
- computed_at
- value
- units

Optional fields MAY include:

- inputs (references to transaction IDs or artifact refs)
- notes

---

## 7. Primitive Signals

Primitive signals are derived directly from evidence within a single domain.

Examples:

- documentation_age_days
- deployment_lead_time
- security_exposure_duration
- test_failure_rate
- technology_version_drift

Primitive signals MUST declare:

- Domain of origin
- Input evidence types
- Computation formula (referenced, not embedded in signal record)

Primitive signals MUST NOT embed full evidence artifacts.

---

## 8. Composite Signals

Composite signals combine multiple primitives and/or domains.

Examples:

- documentation_health
- operational_coherence
- risk_pressure
- governance_alignment_score

Composite signals MUST declare:

- All input signal names
- All input domains
- Weighting or classification strategy
- Policy version used

Composite signals MAY evolve independently of primitive definitions.

---

## 9. Policy Versioning

Signal computation rules MUST reference a policy_version.

Policy version changes MAY alter:

- Thresholds
- Weightings
- Classification boundaries
- Aggregation methods

Policy changes MUST NOT alter historical evidence.

Signals MAY be recomputed retroactively under new policy versions.

---

## 10. Cross-Domain Correlation

Signals MAY incorporate evidence across domains.

Examples:

- High deployment velocity + stale documentation
- Internet-facing + PII + high security exposure
- High change mass + low test coverage

Cross-domain signals MUST:

- Declare input_domains explicitly
- Be reproducible from referenced inputs
- Avoid implicit inference

---

## 11. Determinism Requirements

Given:

- Identical entity envelope
- Identical transaction log
- Identical evidence artifacts
- Identical policy_version

Signal outputs MUST be identical.

Signal computation MUST NOT depend on hidden state.

---

## 12. Storage and Recalculation

Signals MAY be:

- Stored as materialized views
- Computed on demand
- Persisted as panel artifacts

Stored signals MUST be treated as derived artifacts and MUST remain recomputable.

---

## 13. Constraints

- Signals MUST NOT contain secrets.
- Signals MUST NOT embed full documentation or evidence content.
- Signals MUST reference policy versions explicitly.
- Signals MUST remain bounded in size.

---

## 14. Conformance

An implementation conforms to this specification if:

- It computes signals deterministically from defined inputs.
- It references policy versions.
- It maintains reproducibility.
- It does not overwrite or mutate evidence.

---

End of Specification.
