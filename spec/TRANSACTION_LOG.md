# TRANSACTION_LOG_SPEC.md

# Decision Physics
## Transaction Log Specification
Version: 0.1 (Draft)

---

## 1. Status

This document defines the normative structure and constraints for the Decision Physics Transaction Log.

This specification is intended for public review and implementation.

---

## 2. Abstract

The Transaction Log is an append-only ledger of change events for IT system entities and their associated panels/signals.

Each transaction records:
- what changed
- when it changed
- who/what initiated the change
- how it relates to other changes
- optional pointers to before/after materialized artifacts

Transactions MUST validate against the published JSON Schema.

---

## 3. Scope

This specification applies to IT system entities, including:
- services
- applications
- platforms
- components

Non-IT entities are out of scope for version 0.1.

---

## 4. Terminology

**Transaction**  
A single change event represented as one JSON object and stored as one line in JSONL.

**Occurred Time (occurred_at)**  
The time the change occurred (event time).

**Ingested Time (ingested_at)**  
The time the transaction was recorded (ingest time).

**Domain**  
The category of system concern the transaction applies to (e.g., deployment, security).

**Materialized Artifact**  
A stored document/view representing a state snapshot (e.g., metadata.json).  
References to these artifacts MAY be recorded via before_ref and after_ref.

---

## 5. Transaction Record Structure

A transaction MUST contain the following fields:

- txn_id
- occurred_at
- ingested_at
- entity_id
- entity_type
- domain
- operation
- summary
- payload

Optional fields MAY include provenance, correlation, and artifact references.

---

## 6. Field Definitions

### 6.1 txn_id (REQUIRED)
A globally unique identifier (UUID) for the transaction.

### 6.2 occurred_at (REQUIRED)
Timestamp indicating when the change occurred (event time).

### 6.3 ingested_at (REQUIRED)
Timestamp indicating when the transaction was recorded (ingest time).

### 6.4 entity_id (REQUIRED)
The stable identifier of the entity this transaction applies to.

### 6.5 entity_type (REQUIRED)
Enumerated value:
- service
- application
- platform
- component

### 6.6 domain (REQUIRED)
Enumerated value:
- metadata
- deployment
- technology
- security
- documentation
- testing
- signals

### 6.7 operation (REQUIRED)
Enumerated value:
- create
- update
- delete

### 6.8 summary (REQUIRED)
A concise human-readable description of the change.

### 6.9 payload (REQUIRED)
A JSON-serializable object containing type-specific details for the transaction.

In v0.1, payload is intentionally unconstrained beyond being a JSON object.

---

## 7. Optional Provenance Fields

### 7.1 actor (OPTIONAL)
Identifier of the person or service principal responsible for the change.

### 7.2 source (OPTIONAL)
Name of the emitting system/collector (e.g., spinnaker-hook, dependabot, security-scan).

### 7.3 commit_sha (OPTIONAL)
Git commit SHA, when applicable.

### 7.4 idempotency_key (OPTIONAL)
Stable key used to deduplicate repeated emissions of the same semantic event.

---

## 8. Optional Correlation and Causality Fields

### 8.1 correlation_id (OPTIONAL)
Identifier used to associate multiple transactions with one workflow execution (pipeline run, collector run, job run).

### 8.2 caused_by_txn_id (OPTIONAL)
Identifier of a parent transaction that directly caused this transaction (useful for derived signals).

---

## 9. Optional State Reference Fields

### 9.1 before_ref (OPTIONAL)
Reference to the materialized artifact version before the change (e.g., S3 version ID or object reference).

### 9.2 after_ref (OPTIONAL)
Reference to the materialized artifact version after the change.

State references MUST be treated as optimization hints only.
Canonical truth is the append-only transaction sequence.

---

## 10. Constraints

The following constraints MUST be enforced:

- The log MUST be append-only.
- Each JSONL line MUST be one valid transaction object.
- txn_id MUST be globally unique.
- occurred_at MUST represent event time, not ingest time.
- ingested_at MUST be recorded by the writing system.
- domain and operation MUST conform to enumerated values.
- Transactions MUST validate against the published JSON Schema.
- payload MUST be JSON-serializable and bounded (implementations MUST avoid embedding secrets).

---

## 11. Conformance

An implementation conforms to this specification if:

- It emits transaction records that validate against the schema.
- It preserves append-only semantics (no in-place edits).
- It records occurred_at and ingested_at distinctly.
- It provides deterministic replay for entity-level reconstruction.

---

End of Specification.
