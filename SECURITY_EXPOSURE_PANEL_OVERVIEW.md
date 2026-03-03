# SECURITY_EXPOSURE_PANEL_OVERVIEW.md

# Security Exposure Panel — Overview (Decision Physics)

## 1. Purpose

The Security Exposure Panel is a derived, time-bounded posture view for an IT entity. It summarizes vulnerability exposure counts by environment and provides deterministic references to supporting evidence artifacts.

The panel is intended to:

- Provide a concise, comparable view of vulnerability posture across environments
- Support trend and drift analysis via periodized snapshots
- Avoid embedding raw evidence while remaining traceable to it

This panel is a derived artifact. Evidence remains canonical.

---

## 2. Scope

The panel is scoped by:

- A time period (e.g., daily, weekly)
- A set of environments (e.g., qa, stage, prod)
- A set of signal families (e.g., counts, new findings, resolved findings)

Findings are reported **per environment**.

---

## 3. Inputs

The panel is computed from security evidence collected for the entity (e.g., scanner snapshots, vulnerability inventories). Supporting artifacts are referenced via `evidence.summary_ref` and `evidence.latest_ref`.

The panel may be recomputed for the same period if evidence inputs change (e.g., late-arriving scan results). Recomputed outputs should be captured as new materializations.

---

## 4. Output

The panel includes:

- Period and scope metadata
- Last scanned timestamp
- Counts per environment:
  - total findings
  - critical/high findings
  - new critical/high findings
  - resolved critical/high findings
- Derived totals aggregated across environments
- Evidence references for deterministic retrieval of supporting artifacts

---

## 5. Determinism and Traceability

- The panel MUST be reproducible from its referenced evidence inputs and policy definitions.
- The panel MUST remain bounded in size and MUST NOT embed full raw vulnerability detail.
- Evidence references MUST allow deterministic retrieval of the source artifacts used during computation.

---
