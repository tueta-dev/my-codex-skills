---
name: domain-design
description: Use when asked to design the domain model (conceptual design) for a product/feature. Define ubiquitous language, bounded contexts, aggregates/entities/value objects, invariants, domain events, and core use-cases. Produce a strict domain design document and a machine-readable JSON summary. Do not design database schemas, UI, or infrastructure.
---

# Domain Design (Conceptual / DDD)

## Purpose
Create a conceptual domain design that clarifies:
- ubiquitous language (terms and definitions)
- bounded contexts and their relationships (context map)
- aggregates (entities/value objects), responsibilities, and invariants
- domain events and key state transitions
- core use-cases and rules (business logic)

This skill must produce artifacts that other skills can consume (db/app/ui/security).

## When to use
Trigger when the user asks to:
- "ドメイン設計（概念設計）したい"
- "用語や概念を整理したい"
- "境界づけ（BC）や集約を決めたい"
- "ビジネスルール/不変条件を整理したい"
- "ドメインイベントを整理したい"

## Non-goals (must NOT do)
- Do NOT determine design_id. It must be provided by design-orchestrator.
- Do NOT design database tables/columns/indexes.
- Do NOT choose frameworks, libraries, or infrastructure.
- Do NOT design UI screens/components.
- Do NOT write implementation code.
- If a user requests DB/UI/architecture outputs, suggest switching to the appropriate skill.

## Inputs (use what is available)
- Product/feature goal, target users
- Main user journeys / use-cases
- Key business rules and constraints (even partial)
- Existing as-is info (optional): `docs/as-is/as-is-YYYY-MM-DD.md`
  If missing, proceed with best-effort and list Open Questions.

## Artifact policy (1 Skill = 1 Artifact)
### Produces (must)
- `docs/design/<design_id>/domain-design.md`

The file must be deterministic and self-contained.
If the file exists, overwrite it completely.

### Consumes (optional)
- `docs/as-is/as-is-YYYY-MM-DD.md`
- requirement docs / PRDs / tickets
- existing glossary / business docs

## Evidence & assumptions policy
- Facts: explicitly provided by the user or present in consumed artifacts.
- Assumptions: anything not confirmed; label as "Assumption" and include confidence (low/med/high).
- Open Questions: missing decisions that block downstream design.

## Required workflow (do not skip)
1. Normalize the domain goal in one sentence.
2. Build ubiquitous language glossary (terms, synonyms, anti-terms).
3. Identify bounded contexts and create a context map (high-level).
4. Define aggregates:
    - entities, value objects, ids
    - invariants (must always hold)
    - lifecycle/state transitions
5. Define domain events (when/why emitted; payload sketch).
6. Map core use-cases to domain operations and rules.
7. List policies/business rules and edge cases.
8. Produce strict Markdown doc + JSON summary.

---

# STRICT OUTPUT FORMAT (docs/design/<design_id>/domain-design.md)

# Domain Design

## 1. Normalized Goal
- Goal (one sentence):
- Target users:
- Out of scope:

## 2. Ubiquitous Language (Glossary)
Provide a concise glossary.

| Term | Definition | Synonyms | Not / Anti-terms | Notes |
|------|------------|----------|------------------|-------|

## 3. Bounded Contexts
### 3.1 Context List
- Context A:
    - Responsibility:
    - Key terms:
- Context B: ...

### 3.2 Context Map (Text)
Describe relationships:
- A -> B: <relationship type> (e.g., upstream/downstream, conformist, ACL)
- Shared kernel (if any):
- Integration points (domain-level, not technical):

## 4. Aggregates & Domain Model
### 4.1 Aggregate Summary
For each aggregate:
- Aggregate: <name>
    - Purpose:
    - Entity/VO list:
    - Invariants (must always hold):
    - State (if any):
    - Commands (domain operations):
    - Domain events emitted:

### 4.2 Entities / Value Objects
List key fields conceptually (no DB types), focusing on meaning.

- Entity: <name>
    - Identity:
    - Attributes (conceptual):
    - Relationships (conceptual):
- Value Object: <name>
    - Definition:
    - Equality rule:

## 5. Domain Events
For each event:
- Event: <name>
    - Trigger:
    - Meaning:
    - Minimal payload (conceptual):
    - Consumers (domain-level):
    - Idempotency note (conceptual):

## 6. Use-cases to Domain Operations
Map main use-cases to domain commands and rules.

- Use-case: <name>
    - Actor:
    - Preconditions:
    - Main flow (bullets):
    - Domain operations (commands):
    - Rules/Policies applied:
    - Postconditions:
    - Failure cases:

## 7. Rules / Policies / Edge Cases
- Rules (hard constraints):
- Policies (contextual decisions):
- Edge cases:

## 8. Open Questions
Prioritize missing information:
- (High) ...
- (Medium) ...
- (Low) ...

## 9. Assumptions
List assumptions explicitly:
- Assumption: ...
    - Reason:
    - Confidence: low|medium|high

## 10. Structured Summary (Machine-readable JSON)
```json
{
  "design_id": "DP-YYYYMMDD-<scope>-<slug>",
  "artifact": "domain-design",
  "source_refs": {
    "as_is": "docs/as-is/as-is-YYYY-MM-DD.md",
    "design_plan": "docs/design/<design_id>/design-plan.md"
  },
  "goal": "",
  "contexts": [
    {
      "name": "",
      "responsibility": "",
      "key_terms": [],
      "relationships": []
    }
  ],
  "glossary": [
    {
      "term": "",
      "definition": "",
      "synonyms": [],
      "anti_terms": [],
      "notes": ""
    }
  ],
  "aggregates": [
    {
      "name": "",
      "purpose": "",
      "entities": [
        { "name": "", "identity": "", "attributes": [], "relationships": [] }
      ],
      "value_objects": [
        { "name": "", "definition": "", "equality_rule": "" }
      ],
      "invariants": [],
      "states": [],
      "commands": [],
      "events_emitted": []
    }
  ],
  "events": [
    {
      "name": "",
      "trigger": "",
      "meaning": "",
      "payload": [],
      "consumers": [],
      "idempotency_note": ""
    }
  ],
  "use_cases": [
    {
      "name": "",
      "actor": "",
      "preconditions": [],
      "main_flow": [],
      "domain_operations": [],
      "rules": [],
      "postconditions": [],
      "failure_cases": []
    }
  ],
  "open_questions": [
    { "text": "", "priority": "high|medium|low" }
  ],
  "assumptions": [
    { "text": "", "reason": "", "confidence": "low|medium|high" }
  ]
}
```

## Quality bar / Checklist

- [ ] No DB schema / UI / infra details included
- [ ] Glossary present and terms consistent
- [ ] Context boundaries are explicit
- [ ] Aggregates have invariants and commands
- [ ] Events are meaningful and tied to state transitions
- [ ] Use-cases mapped to domain operations
- [ ] Open questions and assumptions are explicit 
- [ ] JSON is valid and matches the schema shape
