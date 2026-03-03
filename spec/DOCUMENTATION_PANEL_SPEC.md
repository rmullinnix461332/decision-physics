# Decision Physics
## Documentation Panel Specification
Version: 0.1 (Draft)

---

## 1. Status

This document defines the normative structure and computation requirements for the Documentation Panel within the Decision Physics framework.

This specification is intended for public review and implementation.

---

## 2. Abstract

The Documentation Panel provides a structured, computed view of documentation completeness and posture for an IT entity.

The panel is derived from:

- content.json (Documentation Content Index)
- Organizational documentation policy rules (required / recommended / nice-to-have definitions)

The Documentation Panel does not store authoritative documentation content.  
It represents a computed posture view.

---

## 3. Scope

This specification applies to documentation associated with IT system entities, including:

- Services
- Applications
- Platforms
- Components

Non-IT entities are out of scope for version 0.1.

---

## 4. Terminology

**Content Index**  
The structured listing of documentation artifacts (content.json).

**Required Document**  
A document type mandated by organizational policy.

**Recommended Document**  
A document type encouraged but not mandatory.

**Nice-to-Have Document**  
A document type that enhances quality but is not required.

**Completeness Metric**  
A computed percentage representing coverage of required and recommended documentation.

---

## 5. Panel Structure

A Documentation Panel MUST contain the following fields:

- schema_name
- schema_version
- entity_id
- computed_at
- metrics
- required_documents
- recommended_documents
- nice_to_have_documents
- missing_required
- missing_recommended

The panel MUST validate against the published JSON Schema.

---

## 6. Computation Rules

### 6.1 Required and Recommended Expectations

Organizational policy MUST define:

- A set of required document keys
- A set of recommended document keys
- Optional nice-to-have document keys

The Content Index MUST be evaluated against these expectations.

---

### 6.2 Coverage Determination

A document expectation is considered "covered" if:

- A corresponding document exists in content.json
- The document has a valid MD5 hash
- The document kind aligns with its classification

Implementations MAY include additional validation rules (e.g., freshness thresholds).

---

### 6.3 Completeness Metric

Completeness percent SHOULD be calculated as:

    (required_covered / required_total) * 100

Implementations MAY optionally include recommended coverage in a secondary metric.

Completeness MUST NOT exceed 100.

---

### 6.4 Missing Documents

missing_required MUST list required document keys not satisfied by the Content Index.

missing_recommended MUST list recommended document keys not satisfied.

Each missing entry MUST include:

- doc_key
- reason

Reasons may include:

- not_found
- invalid_hash
- classification_mismatch
- other_policy_violation

---

## 7. Determinism

The Documentation Panel MUST be:

- Deterministically computable from content.json and policy definitions
- Reproducible given identical inputs
- Free of inferred content not traceable to documented evidence

---

## 8. Constraints

- The panel MUST NOT embed full document content.
- The panel MUST NOT duplicate content.json entries.
- The panel MUST include computed_at timestamp.
- The panel MUST include schema_name and schema_version.

---

## 9. Conformance

An implementation conforms to this specification if:

- It computes coverage strictly from content.json and policy rules.
- It emits panel objects validating against the JSON Schema.
- It maintains deterministic reproducibility.

---

End of Specification.
