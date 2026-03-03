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

1. Entity Layer  
   Canonical representation of “what the thing is.”

2. Evidence Layer  
   Append-only and traceable records of “what happened” and “what is observed.”

3. Panel Layer  
   Deterministic, bounded posture views derived from evidence.

4. Signal Layer  
   Policy-versioned interpretations computed from panels and evidence.

5. Agent Layer  
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

4.1 Collection  
Collectors write evidence artifacts and emit transaction records indicating updates and lineage.

4.2 Curation  
Curation produces bounded, deterministic panel artifacts from evidence inputs, with references preserved.

4.3 Computation  
Signal computation applies policy-versioned interpretation to panels/evidence.

4.4 Consumption  
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

  subgraph Entity_Layer
    E["Base Entity Envelope<br/>(entity.json)"]
  end

  subgraph Evidence_Layer
    TL["Transaction Log<br/>(JSONL in S3)"]
    EV["Domain Evidence Artifacts<br/>(security scans, doc sources,<br/>deployment records, tech inventory)"]
  end

  subgraph Panel_Layer
    SP["Security Exposure Panel"]
    DP["Documentation Panel"]
    TP["Technology Drift Panel<br/>(future)"]
    TST["Test Posture Panel<br/>(future)"]
  end

  subgraph Signal_Layer
    PS["Primitive Signals<br/>(e.g., weighted_exposure,<br/>doc_age_days)"]
    CS["Composite Signals<br/>(e.g., risk_score,<br/>documentation_health)"]
    POL["Policy Definitions<br/>(versioned thresholds, weights)"]
  end

  subgraph Agent_Layer
    KB["Shared Truth Knowledge Base<br/>(index over entity + panels + refs)"]
    AG["Role-Based Agents<br/>(Security, Docs, Delivery, Arch)"]
    TOOLS["Deterministic Tools<br/>(fetch evidence refs, query panels,<br/>resolve provenance)"]
    H["Humans<br/>(approval, decisions, accountability)"]
  end

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

  POL --> PS
  POL --> CS
  PS --> CS

  E --> KB
  TL --> KB
  EV --> KB
  SP --> KB
  DP --> KB
  TP --> KB
  TST --> KB

  KB --> AG
  AG --> TOOLS

  TOOLS --> EV
  TOOLS --> TL
  TOOLS --> SP
  TOOLS --> DP

  AG --> H
  H --> AG
