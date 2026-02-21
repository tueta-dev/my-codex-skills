---
name: design-review
description: Use when asked to review and validate multi-layer design artifacts. Read docs/as-is/as-is-YYYY-MM-DD.md, docs/design/<design_id>/domain-design.md, docs/design/<design_id>/db-design.md, and any other design docs, then produce a structured review highlighting inconsistencies, missing requirements, risks, and prioritized fixes. Do not create new design solutions; focus on review findings and actionable remediation tasks.
---

# Design Review (Cross-layer Consistency)

## Purpose
Review existing design artifacts to ensure:
- cross-layer consistency (domain <-> db <-> app <-> ui <-> arch <-> security)
- completeness (no missing critical requirements)
- implementability (clear next steps, no contradictions)
- risk visibility (security/perf/ops/data integrity)

This is a reviewer skill: it diagnoses and prescribes corrective actions, but does not produce full new designs.

## When to use
Trigger when the user asks to:
- "設計レビューして"
- "整合性チェックして"
- "抜け漏れない？"
- "この設計で実装に進める？"
- "複数ドキュメントを統合して問題点を出して"

## Non-goals (must NOT do)
- Do NOT determine design_id. It must be provided by design-orchestrator.
- Do NOT create a new to-be design from scratch.
- Do NOT output full DB schemas, UI wireframes, or architecture diagrams.
- Do NOT modify code or other design docs; only output the review artifact.
- If a design is missing entirely, report it as a gap and request running the relevant design skill.

## Inputs (consume what exists)
Preferred inputs (if present):
- `docs/as-is/as-is-YYYY-MM-DD.md`
- `docs/design/<design_id>/domain-design.md`
- `docs/design/<design_id>/db-design.md`
  Optional:
- `docs/app-design.md`
- `docs/ui-design.md`
- `docs/architecture-design.md`
- `docs/security-design.md`
- `docs/design/<design_id>/design-plan.md`

If some are missing, review what exists and list gaps.

## Artifact policy (1 Skill = 1 Artifact)
### Produces (must)
- `docs/design/<design_id>/design-review.md`

Overwrite completely if exists.

### Consumes (optional)
- the design artifacts above

## Review philosophy
- Separate Facts vs Inferences:
    - Fact: directly supported by the design docs
    - Inference: reviewer interpretation; include reason + confidence
- Prefer "findings + evidence + impact + recommendation" format.
- All recommendations should be actionable and scoped (what doc to update, what decision is needed).

## Required workflow (do not skip)
1. Inventory artifacts: what exists / missing.
2. Extract claimed requirements and decisions from artifacts.
3. Run cross-layer checks:
    - Domain ↔ DB
    - Domain ↔ UI
    - Domain ↔ App
    - Arch/Ops ↔ Non-functional needs
    - Security ↔ Data/Flows
4. Identify contradictions, gaps, and unclear decisions.
5. Prioritize issues (P0/P1/P2) with rationale.
6. Output review doc (strict format) + JSON summary.

---

# STRICT OUTPUT FORMAT (docs/design/<design_id>/design-review.md)

# Design Review

## 1. Review Scope
- Goal under review:
- Artifacts found:
    - [ ] docs/as-is/as-is-YYYY-MM-DD.md
    - [ ] docs/design/<design_id>/domain-design.md
    - [ ] docs/design/<design_id>/db-design.md
    - [ ] docs/app-design.md
    - [ ] docs/ui-design.md
    - [ ] docs/architecture-design.md
    - [ ] docs/security-design.md
    - [ ] docs/design/<design_id>/design-plan.md
- Missing artifacts (gaps):
- Assumptions (review-time):
    - Assumption: ...
        - Reason:
        - Confidence: low|medium|high

## 2. Executive Summary
- Overall readiness: <ready|needs-work|blocked>
- Top risks (3-7 bullets):
- Recommended next actions (3-7 bullets):

## 3. Cross-layer Findings (prioritized)
Use the following structure for each finding:

### Finding <ID>: <title> (Priority: P0|P1|P2)
- Type: <gap|contradiction|ambiguity|risk>
- Evidence:
    - <quote/section reference> (from which artifact)
- Impact:
    - <what can go wrong>
- Recommendation (actionable):
    - Update: <which doc> / Decide: <what decision> / Clarify: <what needs clarification>
- Confidence: low|medium|high

Repeat for all findings.

## 4. Checklists & Results

### 4.1 Domain ↔ DB
- [ ] Aggregates map cleanly to tables (no missing entity)
- [ ] Invariants are enforceable (DB constraints or app transaction strategy)
- [ ] Keys/identities are consistent (IDs, uniqueness)
- [ ] Event/outbox needs identified if events exist

### 4.2 Domain ↔ UI
- [ ] Use-cases have corresponding screens/states (if UI doc exists)
- [ ] Error states and validation rules surfaced
- [ ] Terminology consistent with glossary

### 4.3 Domain ↔ App
- [ ] Use-cases map to app operations
- [ ] Transaction boundaries align with invariants
- [ ] Failure cases are handled

### 4.4 Arch/Ops ↔ Non-functional
- [ ] Performance assumptions stated and supported
- [ ] Observability/logging plan exists for critical flows
- [ ] Deploy/rollback constraints considered

### 4.5 Security ↔ Data/Flows
- [ ] AuthN/AuthZ decisions exist for each use-case
- [ ] Data classification and access rules considered
- [ ] Audit logging needs identified

## 5. Consolidated Questions (ask user once)
- (High) ...
- (Medium) ...
- (Low) ...

## 6. Remediation Plan (Doc-by-doc)
List concrete updates needed per artifact:

- docs/design/<design_id>/domain-design.md:
    - [ ] ...
- docs/design/<design_id>/db-design.md:
    - [ ] ...
- docs/ui-design.md:
    - [ ] ...
- docs/architecture-design.md:
    - [ ] ...
- docs/security-design.md:
    - [ ] ...

## 7. Structured Summary (Machine-readable JSON)
```json
{
  "design_id": "DP-YYYYMMDD-<scope>-<slug>",
  "artifact": "design-review",
  "source_refs": {
    "as_is": "docs/as-is/as-is-YYYY-MM-DD.md",
    "design_plan": "docs/design/<design_id>/design-plan.md",
    "domain_design": "docs/design/<design_id>/domain-design.md",
    "db_design": "docs/design/<design_id>/db-design.md",
    "app_design": "docs/app-design.md",
    "ui_design": "docs/ui-design.md",
    "architecture_design": "docs/architecture-design.md",
    "security_design": "docs/security-design.md"
  },
  "overall_readiness": "ready|needs-work|blocked",
  "artifacts": {
    "as_is": false,
    "domain": false,
    "db": false,
    "app": false,
    "ui": false,
    "architecture": false,
    "security": false,
    "design_plan": false
  },
  "assumptions": [
    { "text": "", "reason": "", "confidence": "low|medium|high" }
  ],
  "findings": [
    {
      "id": "F1",
      "title": "",
      "priority": "P0|P1|P2",
      "type": "gap|contradiction|ambiguity|risk",
      "evidence": [
        { "artifact": "docs/design/<design_id>/domain-design.md", "ref": "section/path", "note": "" }
      ],
      "impact": [],
      "recommendations": [
        { "action": "update|decide|clarify|run-skill", "target": "docs/...", "detail": "" }
      ],
      "confidence": "low|medium|high"
    }
  ],
  "questions": [
    { "text": "", "priority": "high|medium|low" }
  ],
  "remediation": [
    { "artifact": "docs/design/<design_id>/db-design.md", "tasks": ["..."] }
  ]
}
```

## Quality bar / Checklist

- [ ] No new design produced; only review outputs
- [ ] Findings are prioritized and actionable
- [ ] Evidence is referenced per finding
- [ ] Consolidated questions exist
- [ ] Remediation plan is doc-by-doc
- [ ] JSON is valid and matches schema shape
