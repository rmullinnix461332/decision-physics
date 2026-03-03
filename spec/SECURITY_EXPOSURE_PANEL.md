# SECURITY_EXPOSURE_PANEL_SPEC.md

# Decision Physics
## Security Exposure Panel Specification
Version: 0.1 (Draft)

---

## 1. Status

This document defines the normative structure and constraints for the Security Exposure Panel within the Decision Physics framework.

This specification is intended for public review and implementation.

---

## 2. Abstract

The Security Exposure Panel provides a derived summary of vulnerability exposure for an IT entity, segmented by environment and bounded by a defined period.

The panel includes:

- Period and scope
- Per-environment vulnerability counts (total; critical/high/medium/low; new; resolved)
- Derived totals aggregated across environments
- Deterministic references to supporting evidence artifacts

The panel is non-authoritative and recomputable. Evidence remains canonical.

---

## 3. Scope

This specification applies to IT system entities, including:

- Services
- Applications
- Platforms
- Components

The panel reports findings **per environment** (e.g., qa, stage, prod).

---

## 4. Terminology

Finding  
A vulnerability instance recorded in the underlying evidence sources.

Derived  
A value computed from evidence, not recorded as canonical fact.

Evidence Reference  
A stable reference to a stored artifact that supports deterministic retrieval (e.g., S3 object reference).

Period  
A bounded time window for which the panel snapshot is computed.

---

## 5. Panel Structure

A Security Exposure Panel MUST contain:

- schema_name
- schema_version
- generated_at
- uploaded_at
- period
- scope
- last_scanned
- findings_by_environment
- evidence
- total_derived
- critical_derived
- high_derived
- medium_derived
- low_derived
- resolved_total_derived
- resolved_critical_derived
- resolved_high_derived
- resolved_medium_derived
- resolved_low_derived

The panel MUST validate against the published JSON Schema.

---

## 6. Field Definitions

### 6.1 schema_name (REQUIRED)
Constant identifier for this panel schema.

### 6.2 schema_version (REQUIRED)
Semantic version identifier for this schema.

### 6.3 generated_at (REQUIRED)
Timestamp indicating when the panel was generated (compute time).

### 6.4 uploaded_at (REQUIRED)
Timestamp indicating when the panel was uploaded/persisted (ingest time).

### 6.5 period (REQUIRED)
A period descriptor including:

- id
- type
- start_date
- end_date

### 6.6 scope (REQUIRED)
A scope descriptor including:

- environments[]
- signals[]

### 6.7 last_scanned (REQUIRED)
Timestamp of the most recent scan input considered during computation.

### 6.8 findings_by_environment (REQUIRED)
A map keyed by environment name. Each entry MUST conform to the Vulnerabilities Count structure.

Environment keys MUST correspond to scope.environments.

### 6.9 evidence (REQUIRED)
Evidence references supporting deterministic retrieval of inputs and summaries.

At minimum, evidence MUST include:

- summary_ref
- latest_ref

Implementations MAY extend evidence references in future schema versions.

### 6.10 derived totals (REQUIRED)
The following derived totals MUST be included and represent aggregation across all environments:

- total_derived
- critical_derived
- high_derived
- medium_derived
- low_derived

In addition, resolved derived totals MUST be included:

- resolved_total_derived
- resolved_critical_derived
- resolved_high_derived
- resolved_medium_derived
- resolved_low_derived

Derived totals MUST be consistent with per-environment totals.

---

## 7. Constraints

- Findings MUST be reported per environment.
- findings_by_environment keys MUST be a subset of scope.environments.
- The panel MUST remain bounded in size and MUST NOT embed raw vulnerability inventories.
- Evidence references MUST be stable and resolvable.
- Derived totals MUST be computable by aggregating per-environment values.

---

## 8. Determinism

Given identical evidence inputs and policy rules, the panel output MUST be reproducible.

The panel MUST NOT depend on hidden state.

---

## 9. Conformance

An implementation conforms to this specification if:

- It emits panel artifacts validating against the JSON Schema.
- It reports findings per environment as defined.
- It provides evidence references enabling deterministic retrieval.
- It produces derived totals consistent with environment-level counts.

---

End of Specification.
