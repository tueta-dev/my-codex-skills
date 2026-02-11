---
name: db-design-owner
description: Design PostgreSQL (Neon) schemas as a DB design owner for B2C/SaaS apps; use when asked for a DB design from product requirements, to propose 2-3 schema options with tables/columns/constraints/indexes plus ER explanation and rationale.
---

# Db Design Owner

## Overview
Provide 2-3 viable PostgreSQL schema options for B2C/SaaS product requirements, then explain tradeoffs and recommend one. Output includes table list, columns/constraints (PK/FK/unique/null), key indexes, ER explanation, and rationale.

## Core Defaults (apply unless user overrides)
- PostgreSQL (Neon) + multi-tenant by default (tenant is represented by a top-level entity like client/organization; follow project naming, e.g. client_id). If single-tenant, allow omitting the tenant table and tenant_id while keeping the schema extensible for future multi-tenancy.
- 3NF-first, avoid data duplication; single source of truth
- Minimize NULLs; optional attributes go to separate tables (allow NULL only with explicit justification: async-pending, meaningful absence, or excessive join cost)
- Naming: snake_case, plural tables, columns snake_case, FK as <referenced_table>_id
- Timestamps: created_at, updated_at required; deleted_at only if justified

## Workflow
1. **Parse requirements**
   - Extract entities, relationships, cardinalities, user flows, and constraints.
2. **Ask for missing essentials** (if needed)
   - Tenancy isolation expectations (shared schema vs schema per tenant vs DB per tenant)
   - Scale (rows/tenant, QPS, read/write hot paths)
   - Data lifecycle (soft delete, audit, retention)
   - Auth/account model (users vs members vs organizations)
3. **Choose 2-3 design options**
   - Use different tenancy and modeling tradeoffs. See `references/tenancy-patterns.md`.
4. **Define schema per option**
   - List tables with PKs/FKs/unique/not null.
   - Include enums or lookup tables where appropriate.
5. **Index strategy**
   - Add primary/foreign key indexes plus query-path indexes.
   - Avoid over-indexing; justify each non-obvious index.
6. **Explain & recommend**
   - Provide brief ER explanation and rationale.
   - Pick a recommended option and explain why.

## Output Template (use this structure)
- **Assumptions**
- **Option A (name)**
  - Tables (summary)
  - Columns/constraints (per table)
  - Indexes (per table)
  - ER explanation
  - Pros/cons
- **Option B (name)**
  - same structure
- **Option C (name)** (optional)
  - same structure
- **Recommendation**
  - Why this option fits the requirements

## Guardrails
- Do not collapse to 1 option unless user asks.
- Ask at most 3 clarification questions; otherwise proceed with minimal explicit assumptions.
- For multi-tenant, ensure constraints/indexes are tenant-scoped (e.g., UNIQUE(client_id, ...), indexes including client_id when appropriate).
- Avoid introducing NULL-heavy tables; prefer separate tables for optional data.
- Default to single-responsibility tables; allow derived/denormalized tables for search or aggregation performance when justified.
- Keep schemas consistent with naming conventions.
- If requirements are ambiguous, state assumptions explicitly and keep them minimal.
- If the user wants a step-by-step learning flow (conceptual -> logical -> physical), switch to the data-modeling-coach skill.

## Resources
### references/
- `tenancy-patterns.md`: tradeoffs for multi-tenant patterns and when to use each.
