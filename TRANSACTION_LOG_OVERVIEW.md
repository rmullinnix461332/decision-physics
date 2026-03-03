# TRANSACTION_LOG_OVERVIEW.md

# Decision Physics — Transaction Log Overview

## 1. Purpose

The Transaction Log is an append-only record of change events for:

- The Base Entity Envelope (entity metadata)
- Evidence panels and computed signals associated with an entity

It exists to provide **evidence-based memory** that is:

- Durable
- Deterministic
- Reconstructable
- Auditable

The Transaction Log is stored as **JSONL** in S3.

---

## 2. What the Transaction Log Is (and is not)

### 2.1 Is
- A chronological ledger of entity and panel/signal changes
- A source of truth for “what changed, when, and why”
- A replayable stream to reconstruct materialized views (envelopes/panels)

### 2.2 Is Not
- A replacement for the Base Entity Envelope
- A place to store large documents inline (payloads should remain bounded)
- A schema for “AI hooks” (AI integration is out of scope here)

---

## 3. Storage Model (S3 + JSONL)

### 3.1 Format
Each line is one JSON object representing a single transaction.

### 3.2 Recommended S3 Layout
Transactions do not need to be co-located with the Base Entity Envelope.

A recommended partitioning strategy:

- transactions/entity_id=<ENTITY_ID>/yyyy/mm/dd.jsonl

This enables:
- Efficient filtering by entity
- Natural time-bounded replay
- Backfill and reprocessing without rewriting history

---

## 4. Core Design Properties

### 4.1 Append-Only
Transactions are never modified in place.
Corrections are recorded as new transactions.

### 4.2 Event Time vs Ingest Time
- occurred_at: when the change occurred (event time)
- ingested_at: when the transaction was recorded (ingest time)

This supports delayed collectors, replay, and backfill.

### 4.3 Deterministic Reconstruction
Current state can be rebuilt by replaying transactions in order.
Pointers like before_ref/after_ref are optimization hints, not authority.

### 4.4 Idempotency
Collectors may re-emit events.
Transactions support deduplication through idempotency_key.

### 4.5 Causality and Correlation
Transactions support linking:
- Many events to one run/pipeline execution (correlation_id)
- Derived transactions to a parent event (caused_by_txn_id)

---

## 5. Domains and Operations

Transactions are categorized by domain:

- metadata
- deployment
- technology
- security
- documentation
- testing
- signals

Operations define intent:

- create
- update
- delete

---

## 6. Payload Strategy

payload is intentionally typed as `any` in v0.1.

Payload schemas will be refined as:
- panel types are defined
- evidence domains expand
- signal definitions mature

Until then:
- payload MUST remain bounded and JSON-serializable
- payload SHOULD NOT include secrets
- payload SHOULD reference large external artifacts by ref rather than inlining

---

## 7. Relationship to Other Artifacts

- Base Entity Envelope: canonical entity record (materialized view)
- Transaction Log: append-only change ledger
- Evidence Panels / Signals: derived views and computations built from curated evidence and transactions
