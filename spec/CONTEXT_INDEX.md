# Decision Physics
## Documentation Content Index and Concatenation Specification
Version: 0.1 (Draft)

---

## 1. Status

This document defines the normative structure for documentation indexing and AI-oriented concatenation for IT entity documentation.

---

## 2. Abstract

Decision Physics documentation consists of:

- content.json — a structured index of entity documents
- content.md — a concatenated markdown file assembled from indexed markdown documents and AI-oriented front matter

These artifacts support deterministic access, evidence-based memory, and reconstructable knowledge base inputs.

---

## 3. Artifact: content.json

### 3.1 Required Fields

content.json MUST include:

- schema_name
- schema_version
- entity_id
- generated_at
- documents[]

### 3.2 Document Record Requirements

Each document record MUST include:

- doc_id
- title
- kind (required | recommended | nice_to_have)
- source (location reference)
- md5

### 3.3 MD5 Integrity

md5 MUST:

- Be computed from raw document content bytes
- Be encoded as lowercase hexadecimal
- Match the content included in content.md

---

## 4. Artifact: content.md

content.md is an AI-oriented concatenation of markdown documents referenced in content.json.

---

### 4.1 Required YAML Front Matter

content.md MUST begin with YAML front matter.

Example:

    ---
    schema_name: decision-physics.documentation.content-concat
    schema_version: 0.1.0
    entity_id: example-service
    generated_at: 2026-03-02T21:14:00Z
    ordering_rule: kind_then_title
    ai_notes: >
      This file concatenates authoritative markdown documents for this entity.
      Each section includes doc_id and md5 for traceability.
      Prefer citing doc_id and section headings when answering questions.
    ---

Front matter MUST be valid YAML.

---

### 4.2 Deterministic Ordering

content.md MUST declare ordering_rule in front matter.

Acceptable strategies include:

- kind_then_title
- explicit_doc_id_order
- policy_defined_order

Given identical content.json and source documents, content.md MUST be reproducible exactly.

---

### 4.3 Document Separation Requirements

Each embedded document MUST be separated by boundary markers and metadata.

Required structure:

    <!-- BEGIN_DOCUMENT -->
    ## Document: <doc_id>

    - Title: <title>
    - Kind: required | recommended | nice_to_have
    - Source: <repo_type>/<repo_org>/<repo_slug>:<path>@<ref>
    - MD5: <md5>

    <original markdown content here>

    <!-- END_DOCUMENT -->

Requirements:

- BEGIN_DOCUMENT and END_DOCUMENT markers MUST be present.
- MD5 MUST match content.json.
- Source reference MUST align with content.json.
- Document content MUST NOT be modified during concatenation.

---

## 5. Deterministic Reconstruction

An implementation MUST be able to reconstruct content.md from:

- content.json
- referenced source markdown documents
- declared ordering rule

No hidden state is permitted.

---

## 6. Constraints

- content.json MUST validate against its JSON Schema.
- content.md MUST include front matter.
- Every document in content.md MUST exist in content.json.
- Secrets MUST NOT appear in front matter.
- Non-markdown documents MUST NOT be concatenated without format-specific handling.

---

## 7. Conformance

An implementation conforms to this specification if:

- It produces content.json conforming to schema requirements.
- It generates content.md that is reproducible and traceable.
- It includes document separation markers and valid YAML front matter.
- It preserves MD5 integrity verification.

---

End of Specification.
