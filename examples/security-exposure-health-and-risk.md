# Example: Security Exposure Health and Risk Scoring
Decision Physics — Example Artifact  
Version: 0.1 (Illustrative Only)

---

## 1. Purpose

This document provides a worked example of:

- Security Exposure Health (operational posture score)
- Security Risk Score (context-adjusted risk pressure)

These examples demonstrate how signals may be computed from the
Security Exposure Panel and Base Entity Envelope.

This example is illustrative and not normative.

---

## 2. Input Snapshot

From Security Exposure Panel (aggregated across environments):

| Severity | Active Findings |
|----------|-----------------|
| Critical | 2               |
| High     | 4               |
| Medium   | 6               |
| Low      | 10              |

Resolved during period:

| Severity | Resolved Findings |
|----------|-------------------|
| Critical | 1                 |
| High     | 2                 |
| Medium   | 3                 |
| Low      | 5                 |

Entity Context:

- Criticality: mission-critical
- Boundary: internet-facing
- Contains PII: true

---

## 3. Weighted Exposure Calculation

Severity Weights (policy-defined):

| Severity | Weight |
|----------|--------|
| Critical | 10     |
| High     | 6      |
| Medium   | 3      |
| Low      | 1      |

Weighted Exposure:

    (2 × 10) +
    (4 × 6)  +
    (6 × 3)  +
    (10 × 1)

    = 20 + 24 + 18 + 10
    = 72

weighted_exposure = 72

---

## 4. Remediation Ratio

Resolved Weighted:

    (1 × 10) +
    (2 × 6)  +
    (3 × 3)  +
    (5 × 1)

    = 10 + 12 + 9 + 5
    = 36

resolved_weighted = 36

Remediation Ratio:

    36 / (36 + 72)
    = 0.333

remediation_ratio = 0.333

---

## 5. Security Exposure Health (Operational Posture)

Normalization constant (policy-defined):

    exposure_cap = 500

Exposure Score:

    72 / 500 = 0.144

Health Formula (example v1.0):

    health_score =
        100 *
        (
            (1 - exposure_score) * 0.7 +
            remediation_ratio * 0.3
        )

Substituting values:

    100 * ((1 - 0.144)*0.7 + 0.333*0.3)
    = 100 * (0.856*0.7 + 0.333*0.3)
    = 100 * (0.599 + 0.100)
    = 69.9

Security Exposure Health ≈ 70

Interpretation:
Borderline acceptable posture with moderate exposure and moderate remediation velocity.

---

## 6. Context-Adjusted Security Risk Score

Context Multipliers:

| Context            | Multiplier |
|-------------------|------------|
| Mission-Critical  | 1.6        |
| Internet-Facing   | 1.4        |
| Contains PII      | 1.3        |

Combined Multiplier:

    1.6 × 1.4 × 1.3 ≈ 2.91

Base Risk:

    base_risk = weighted_exposure = 72

Context-Adjusted Risk:

    72 × 2.91 = 209.5

Risk normalization cap (policy-defined):

    risk_cap = 800

Risk Score:

    (209.5 / 800) × 100
    = 26

Security Risk Score ≈ 26

Interpretation:
Moderate contextual risk given current exposure levels.
Risk would escalate rapidly with additional critical findings.

---

## 7. Signal Classification

These would be emitted as versioned signals:

Primitive Signals:
- weighted_exposure
- remediation_ratio

Composite Signals:
- security_exposure_health
- security_risk_score

Example Signal Record:

{
  "schema_name": "decision-physics.signal",
  "schema_version": "0.1.0",
  "entity_id": "example-service",
  "signal_name": "security_exposure_health",
  "signal_version": "1.0.0",
  "policy_version": "security-policy-2026-03",
  "input_domains": ["security", "governance", "access"],
  "computed_at": "2026-03-02T22:00:00Z",
  "value": 70,
  "units": "score_0_100"
}

---

## 8. Important Notes

- These calculations are illustrative and policy-dependent.
- Weights and caps MUST be versioned.
- Signals MUST remain recomputable from panel evidence.
- Evidence remains canonical; signals are interpretations.

---

End of Example
