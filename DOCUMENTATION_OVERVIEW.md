# Documentation — Overview (Decision Physics)

## 1. Purpose

Documentation evidence in Decision Physics provides deterministic, reconstructable knowledge associated with an IT entity.

Documentation is represented in two complementary forms:

1) **content.json** — an index of documents associated with an entity  
2) **content.md** — a concatenated, AI-oriented document assembled from source markdown documents

In addition, a **Documentation Panel** summarizes documentation posture using completeness metrics and missing-document lists.

Documentation artifacts are adjacent to the entity representation and are not treated as generic “panels” at generation time; the panel is derived from these artifacts.

---

## 2. Artifacts

### 2.1 content.json (Document Index)

A machine-readable index of all documentation items associated with an entity.

Each item includes:

- document identity
- logical classification (required / recommended / nice-to-have)
- source location
- an MD5 hash for content integrity

The index is suitable for deterministic tools and knowledge base ingestion.

---

### 2.2 content.md (AI-Oriented Concatenation)

A single markdown file created by concatenating source markdown documents referenced by content.json.

content.md MUST include:

- front matter intended for AI consumption
- explicit boundaries between documents
- the MD5 hash of each document
- stable references back to the indexed document identifiers

content.md is designed as an AI-friendly representation of entity documentation while remaining traceable to source documents.

---

### 2.3 Documentation Panel (Derived Posture)

A computed posture view of documentation quality and completeness.

The panel includes:

- document completeness metrics
- required / recommended / nice-to-have document expectations
- missing required documents
- missing recommended documents

The panel is derived from content.json and organizational policy rules.

---

## 3. Integrity and Determinism

- Each document item MUST have a stable identifier and an MD5 hash.
- The content.md concatenation MUST be reproducible from source documents plus deterministic ordering rules.
- Large documents MUST be referenced rather than duplicated across multiple stores.

---

## 4. Scope (v0.1)

Initial scope focuses on markdown-based documentation (md).

Support for additional formats (PDF, HTML) may be added later but must preserve deterministic hashing and indexing.
