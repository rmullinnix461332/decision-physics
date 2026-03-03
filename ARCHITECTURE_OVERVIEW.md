# ARCHITECTURE_OVERVIEW.md

# Decision Physics — Architecture Overview
Version: 0.1 (Draft)

---

## 1. Purpose

This document describes the high-level architecture of Decision Physics.

Decision Physics is an evidence infrastructure that enables deterministic posture views (panels) and policy-versioned interpretations (signals), supporting multi-agent systems that reason over a shared truth substrate using deterministic tools.

This document is descriptive, not normative.

---

## 2. Architectural Summary

Decision Physics is organized into layered concerns:

1. **Entity Layer**  
   Canonical representation of “what the thing is.”

2. **Evidence Layer**  
   Append-only and traceable records of “what happened” and “what is observed.”

3. **Panel Layer**  
   Deterministic, bounded posture views derived from evidence.

4. **Signal Layer**  
   Policy-versioned interpretations computed from panels and evidence.

5. **Agent Layer**  
   Role-based agents operating over a shared truth substrate, using deterministic tools to retrieve detailed evidence.

Evidence remains canonical.  
Panels and signals remain recomputable.

---

## 3. Core Artifacts

### 3.1 Base Entity Envelope
A structured record describing an IT entity, including identity, governance, ownership, location, intelligence boundaries, operations, and access.

The entity envelope is the anchor for all evidence and derived views.

### 3.2 Transaction Log
An append-only JSONL log that records changes across domains (metadata, security, documentation, technology, deployment, testing, signals).

The transaction log supports reconstruction, lineage, and audit.

### 3.3 Domain Evidence Artifacts
Domain-specific evidence is collected systematically and stored as structured artifacts (e.g., vulnerability snapshots, scan outputs, inventory exports). These may be large and are referenced rather than embedded.

### 3.4 Panels
Panels are bounded, deterministic posture views derived from evidence. Examples include:

- Security Exposure Panel
- Documentation Panel
- Technology Drift Panel (future)
- Testing Posture Panel (future)

Panels are intended for readability, comparability, and deterministic interpretation. They do not replace raw evidence.

### 3.5 Signals
Signals are derived interpretations computed from panels and evidence under a declared policy version.

Signals may be:

- Primitive (single-domain derivations)
- Composite (cross-domain correlations)

Signals are non-authoritative and recomputable.

### 3.6 Shared Truth Substrate
All agents share a common knowledge base backed by the evidence substrate (entity + transactions + panels + referenced evidence). Agents do not maintain private divergent truth stores.

If drift occurs, the substrate is treated as the defect.

---

## 4. Data Flow and Responsibility Boundaries

### 4.1 Collection
Collectors write evidence artifacts and emit transaction records indicating updates and lineage.

### 4.2 Curation
Curation produces bounded, deterministic panel artifacts from evidence inputs, with references preserved.

### 4.3 Computation
Signal computation applies policy-versioned interpretation to panels/evidence.

### 4.4 Consumption
Agents and humans consume panels and signals, and use deterministic tools to retrieve supporting evidence details when needed.

---

## 5. Determinism and Recomputability

Given identical:

- Entity envelope
- Transaction log
- Evidence artifacts
- Panel schemas
- Policy versions

Panels and signals must be reproducible.

No hidden state is permitted in panel/signal computation.

---

## 6. Mermaid Diagram

```mermaid
flowchart TD
  %% Layers
  subgraph L1[Entity Layer]
    E[Base Entity Envelope\n(entity.json)]
  end

  subgraph L2[Evidence Layer]
    TL[Transaction Log\n(JSONL in S3)]
    EV[Domain Evidence Artifacts\n(security scans, doc sources,\ndeployment records, tech inventory)]
  end

  subgraph L3[Panel Layer]
    SP[Security Exposure Panel]
    DP[Documentation Panel]
    TP[Technology Drift Panel\n(future)]
    TST[Test Posture Panel\n(future)]
  end

  subgraph L4[Signal Layer]
    PS[Primitive Signals\n(e.g., weighted_exposure,\ndoc_age_days)]
    CS[Composite Signals\n(e.g., risk_score,\ndocumentation_health)]
    POL[Policy Definitions\n(versioned thresholds, weights)]
  end

  subgraph L5[Agent Layer]
    KB[Shared Truth Knowledge Base\n(index over entity+panels+refs)]
    AG[Role-Based Agents\n(Security, Docs, Delivery, Arch)]
    TOOLS[Deterministic Tools\n(fetch evidence refs, query panels,\nresolve provenance)]
    H[Humans\n(approval, decisions, accountability)]
  end

  %% Flows
  E --> TL
  EV --> TL

  E --> SP
  EV --> SP
  TL --> SP

  E --> DP
  EV --> DP
  TL --> DP

  E --> TP
  EV --> TP
  TL --> TP

  E --> TST
  EV --> TST
  TL --> TST

  SP --> PS
  DP --> PS
  TP --> PS
  TST --> PS

  PS --> CS
  POL --> PS
  POL --> CS

  E --> KB
  SP --> KB
  DP --> KB
  TP --> KB
  TST --> KB
  TL --> KB
  EV --> KB

  KB --> AG
  AG --> TOOLS
  TOOLS --> EV
  TOOLS --> TL
  TOOLS --> SP
  TOOLS --> DP

  AG --> H
  H --> AG
