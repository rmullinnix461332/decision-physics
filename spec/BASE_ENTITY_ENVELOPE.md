# Decision Physics
## Base Entity Envelope Specification
Version: 0.1 (Draft)

---

## 1. Status

This document defines the normative structure for the Base Entity Envelope within the Decision Physics framework.

This specification is intended for public review and implementation.

---

## 2. Abstract

The Base Entity Envelope defines the canonical representation of an IT system entity.

It establishes required identity, governance, ownership, location, intelligence, access, and operational metadata.

All evidence and signal computation within Decision Physics MUST reference a valid Base Entity Envelope.

---

## 3. Scope

This specification applies to IT systems, including:

- Services
- Applications
- Platforms
- Components

Non-IT entities are out of scope for version 0.1.

---

## 4. Terminology

**Entity**  
A system, service, application, platform, or component managed within IT scope.

**Envelope**  
The structured metadata container describing an entity.

**Governance**  
Control attributes associated with regulatory and operational risk.

**Signal**  
A computed value derived from curated evidence.

---

## 5. Envelope Structure

An Entity Envelope MUST contain the following top-level fields:

- entity_id
- entity_type
- identity
- governance
- ownership
- location
- intelligence
- access
- operations

All fields are required unless explicitly marked optional.

---

## 6. Field Definitions

### 6.1 entity_id

A unique immutable identifier for the entity.

This value MUST remain stable across the entity lifecycle.

---

### 6.2 entity_type

Enumerated value:

- service
- application
- platform
- component

---

### 6.3 identity

Defines canonical catalog identity attributes.

Required fields:

- catalog_id
- display_name
- lifecycle_stage
- created_at

The identity section MUST align with the authoritative system catalog.

---

### 6.4 governance

Defines regulatory and risk classification attributes.

Required fields:

- criticality
- data_classification
- contains_pii

Optional fields:

- regulatory_scope
- control_frameworks
- exception_status

Governance metadata may influence signal weighting.

---

### 6.5 ownership

Defines accountable organizational structures.

Required fields:

- organization
- team

Optional fields:

- business_domain
- system_domain
- primary_contact
- secondary_contact

Ownership determines accountability routing for computed signals.

---

### 6.6 location

Defines source control and artifact location metadata.

Required fields:

- repo_type
- repo_org
- repo_slug

Optional fields:

- default_branch
- deployment_artifacts

Location fields MUST be sufficient for deterministic repository resolution.

---

### 6.7 intelligence

Defines AI interaction boundaries.

Required fields:

- agent_readable
- knowledge_plane

Optional fields:

- intent_boundary
- deterministic_tools
- memory_scope

Intelligence metadata constrains AI system access and inference boundaries.

---

### 6.8 access

Defines system exposure characteristics.

Required fields:

- audience
- boundary

Optional fields:

- methods
- roles_required
- authentication_mechanisms

Access metadata may influence risk signals.

---

### 6.9 operations

Defines operational delivery and runtime metadata.

Required fields:

- delivery_system

Optional fields:

- cluster
- runtime
- foundation_version
- helm_chart_version
- last_deploy_at

Operations metadata links entity representation to deployment evidence.

---

## 7. Constraints

The following constraints MUST be enforced:

- entity_id MUST be immutable.
- Enumerated fields MUST conform to defined value sets.
- Required fields MUST be present.
- Instances MUST validate against the published JSON Schema.
- Governance metadata MUST not be omitted for active systems.

---

## 8. Conformance

An implementation conforms to this specification if:

- Entity instances validate against the official JSON Schema.
- Required fields are present.
- Enumerations are respected.
- Version metadata of the envelope is preserved.

---

End of Specification.
