# Decision Physics
## Documentation Freshness and Health Specification
Version: 0.1 (Draft)

---

## 1. Status

This document defines normative rules for computing:

- Documentation Freshness
- Documentation Health

These metrics are derived from:

- content.json
- Transaction Log (deployment + metadata domains)
- Source control commit history

---

## 2. Definitions

Documentation Freshness  
A measure of how recently documentation was updated relative to system change.

Accumulated Documentation Debt  
A measure of system change occurring without corresponding documentation updates.

Documentation Health  
A composite metric incorporating completeness, freshness, and accumulated debt.

---

## 3. Documentation Freshness

### 3.1 Age-Based Freshness

For each document in content.json:

    document_age_days = (now - last_document_update_timestamp)

If last update timestamp is not directly recorded, it MUST be derived from:

- Git commit history
- Transaction log metadata domain updates

---

### 3.2 Freshness Thresholds

Recommended baseline thresholds (policy-configurable):

Required Documents:
- Fresh: <= 30 days
- Aging: 31–90 days
- Stale: > 90 days

Recommended Documents:
- Fresh: <= 60 days
- Aging: 61–120 days
- Stale: > 120 days

Nice-to-Have:
- Not freshness-gated by default

Implementations MAY override thresholds per document type.

---

### 3.3 Entity-Level Freshness

Entity documentation freshness MAY be computed as:

    max_age_required = maximum document_age_days across required documents

or

    weighted_avg_age = average(document_age_days weighted by classification)

Implementations MUST define which strategy is used and apply it consistently.

---

## 4. Accumulated Documentation Debt

Accumulated Documentation Debt measures system change without documentation updates.

---

### 4.1 Change Mass Definition

Change Mass MAY be computed using:

- Total PR count since last documentation update
- Total LOC changed since last documentation update
- Weighted change score (e.g., LOC + file multiplier)

Change mass MUST be derived from the deployment or metadata transaction domain.

---

### 4.2 Debt Accumulation Window

For each entity:

    last_doc_update_time = max(document update timestamps)
    change_mass_since = sum(change_mass where commit_time > last_doc_update_time)

This value represents documentation drift pressure.

---

### 4.3 Debt Thresholds (Policy Defined)

Example policy:

- Low Debt: <= 5 PRs since last doc update
- Moderate Debt: 6–20 PRs
- High Debt: > 20 PRs

Alternative mass-based thresholds MAY use LOC-based scoring.

Thresholds MUST be documented and versioned.

---

## 5. Documentation Health Metric

Documentation Health is a composite score derived from:

- Completeness (from Documentation Panel)
- Freshness
- Accumulated Documentation Debt

---

### 5.1 Component Scores

Completeness Score (C)
    = completeness_percent (0–100)

Freshness Score (F)
    Derived from document age classification
    Fresh = 100
    Aging = 70
    Stale = 40

Debt Score (D)
    Inverse score based on accumulated change mass

Example:
    Low Debt = 100
    Moderate Debt = 70
    High Debt = 40

---

### 5.2 Composite Health Formula (Example)

    Documentation Health = (C * 0.5) + (F * 0.3) + (D * 0.2)

Weights MUST be declared and versioned in policy configuration.

The composite score MUST be bounded 0–100.

---

## 6. Determinism Requirements

- All metrics MUST be derived from transaction log and content index.
- No manual overrides are permitted without a recorded transaction.
- Threshold definitions MUST be versioned.
- Given identical inputs and policy configuration, health computation MUST be reproducible.

---

## 7. Constraints

- Documentation Health MUST NOT embed document content.
- Health metrics MUST include computed_at timestamp.
- All classification thresholds MUST be explicit and documented.

---

## 8. Conformance

An implementation conforms if:

- It derives freshness strictly from document update timestamps.
- It computes accumulated debt strictly from change mass since last doc update.
- It emits health metrics reproducibly.
- It does not infer undocumented updates.

---

End of Specification.
