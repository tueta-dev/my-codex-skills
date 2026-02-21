---
name: db-design
description: Use when asked to design a relational database schema from domain requirements. Propose 2-3 viable PostgreSQL schema options, explain tradeoffs, recommend one, and provide constraints/indexes plus key query patterns. Consume docs/design/<design_id>/domain-design.md when available. Produce a strict DB design document with optional DDL and a machine-readable JSON summary. Do not design UI or infrastructure.
---

# DB Design (PostgreSQL, 3NF-first)

## Purpose
Design a PostgreSQL schema that supports the domain model and key use-cases.
This skill must:
- propose 2-3 viable schema options
- describe tradeoffs and recommend one
- define tables/columns/constraints/indexes
- outline key query patterns and why indexes exist
- output a deterministic artifact for downstream implementation

## When to use
Trigger when the user asks to:
- "DB設計したい/スキーマを作りたい"
- "テーブル設計・正規化を考えたい"
- "インデックスや制約を含めて設計したい"
- "domain-design から DB に落としたい"

## Non-goals (must NOT do)
- Do NOT determine design_id. It must be provided by design-orchestrator.
- Do NOT design UI or screen flows.
- Do NOT choose infrastructure/deployment architecture.
- Do NOT write application-layer code.
- Do NOT over-optimize without evidence (keep it pragmatic).

## Inputs (use what is available)
- `docs/design/<design_id>/domain-design.md` (preferred)
- key query patterns / access patterns (if known)
- scale assumptions (rows growth, read/write mix) if provided
- multi-tenancy requirements (if any)
  If unknown, state assumptions and list Open Questions.

## Artifact policy (1 Skill = 1 Artifact)
### Produces (must)
- `docs/design/<design_id>/db-design.md`

Overwrite completely if exists.

### Consumes (optional)
- `docs/design/<design_id>/domain-design.md`
- `docs/as-is/as-is-YYYY-MM-DD.md` (for migration/compat constraints)

## Design defaults (apply unless user overrides)
- PostgreSQL
- 3NF-first (avoid duplication; single source of truth)
- Explicit constraints: PK/FK/UNIQUE/CHECK/NOT NULL
- Soft delete only if explicitly required
- Use `timestamptz` for timestamps (conceptual; avoid implementation detail beyond types)
- Prefer surrogate PKs unless domain requires natural keys
- Always include created_at / updated_at (unless user forbids)

## Required workflow (do not skip)
1. Extract entities and relationships from domain-design (or user input).
2. Identify key access patterns (queries) and critical constraints.
3. Create 2-3 schema options:
    - Option A: baseline normalized
    - Option B: variant (tenancy/partitioning/denormalization minimal)
    - Option C: only if materially different
4. For each option:
    - table list with columns (name, meaning, type, nullability)
    - constraints (PK/FK/UNIQUE/CHECK)
    - indexes (with rationale)
    - pros/cons and risks
5. Recommend one option with clear rationale.
6. Provide a consolidated schema spec for the recommended option.
7. Provide key query patterns and index mapping.
8. List Open Questions and Assumptions.
9. Output strict Markdown + JSON summary.

---

# STRICT OUTPUT FORMAT (docs/design/<design_id>/db-design.md)

# DB Design

## 1. Normalized Goal
- Goal (one sentence):
- In scope:
- Out of scope:

## 2. Inputs & Assumptions
### 2.1 Consumed Artifacts
- domain-design: <present/absent>
- as-is: <present/absent>

### 2.2 Assumptions
- Assumption: ...
    - Reason:
    - Confidence: low|medium|high

### 2.3 Open Questions
- (High) ...
- (Medium) ...
- (Low) ...

## 3. Key Access Patterns (Queries)
List the queries the schema must support:
- Q1: <description>
    - Filters:
    - Sort:
    - Joins:
    - Expected cardinality (if known):
- Q2: ...

## 4. Schema Options (2–3 options required)

### Option A: <name>
#### A.1 Tables
For each table:
- Table: <name>
    - Purpose:
    - Columns:
        - <col>: <meaning> (type: <...>, null: Y/N)
    - Constraints:
        - PK:
        - FK:
        - UNIQUE:
        - CHECK:
    - Indexes:
        - <index> (reason: ...)

#### A.2 ER Notes (text)
- Relationships summary:

#### A.3 Tradeoffs
- Pros:
- Cons:
- Risks:

### Option B: <name>
(same structure)

### Option C: <name> (optional)
(same structure)

## 5. Recommendation
- Recommended option: <A|B|C>
- Why this option:
- What we give up:

## 6. Recommended Schema (Consolidated Spec)
This section is the single source of truth for implementation.

### 6.1 Tables (final)
- Table: ...
    - Columns:
    - Constraints:
    - Indexes:

### 6.2 Referential Integrity & Delete/Update Rules
- On delete/update policy per FK (restrict/cascade/set null), with rationale.

### 6.3 Optional: DDL Sketch (PostgreSQL)
Provide a minimal DDL sketch for the recommended option (only if confident).
```sql
-- table definitions here
```

## 7. Index-to-Query Mapping
Map each index to the access patterns it supports:

- Index <...> -> supports Q1, Q3 because ... 
- Index <...> -> supports Q2 because ...

## 8. Risks & Migration Notes
- Risks:
- Migration considerations (if as-is exists):

## 9. Structured Summary (Machine-readable JSON)
```json
{
  "design_id": "DP-YYYYMMDD-<scope>-<slug>",
  "artifact": "db-design",
  "source_refs": {
    "as_is": "docs/as-is/as-is-YYYY-MM-DD.md",
    "design_plan": "docs/design/<design_id>/design-plan.md",
    "domain_design": "docs/design/<design_id>/domain-design.md"
  },
  "goal": "",
  "assumptions": [
    { "text": "", "reason": "", "confidence": "low|medium|high" }
  ],
  "open_questions": [
    { "text": "", "priority": "high|medium|low" }
  ],
  "access_patterns": [
    { "id": "Q1", "description": "", "filters": [], "sort": "", "joins": [] }
  ],
  "options": [
    {
      "id": "A",
      "name": "",
      "tables": [
        {
          "name": "",
          "purpose": "",
          "columns": [
            { "name": "", "meaning": "", "type": "", "nullable": false }
          ],
          "constraints": {
            "pk": [],
            "fk": [],
            "unique": [],
            "check": []
          },
          "indexes": [
            { "name": "", "columns": [], "type": "btree|gin|gist|hash", "reason": "" }
          ]
        }
      ],
      "tradeoffs": { "pros": [], "cons": [], "risks": [] }
    }
  ],
  "recommendation": { "option_id": "A", "why": [], "tradeoffs": [] },
  "final_schema": {
    "tables": [],
    "fk_policies": []
  },
  "index_query_mapping": [
    { "index": "", "supports": ["Q1"], "because": "" }
  ],
  "risks": [],
  "migration_notes": []
}
```

## Quality bar / Checklist

- [ ] 2–3 materially different options included
- [ ] Every option has constraints + indexes + tradeoffs
- [ ] Recommendation is explicit and justified
- [ ] Final consolidated schema exists (single source of truth)
- [ ] Access patterns listed and mapped to indexes
- [ ] Assumptions & Open Questions are explicit 
- [ ] JSON is valid and matches schema shape
