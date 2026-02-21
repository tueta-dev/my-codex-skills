---
name: design-orchestrator
description: Use when asked to plan and coordinate multi-layer design work across skills (domain/db/app/architecture/ui/security). Normalize the goal and scope, select required skills, build an execution DAG (parallel/serial), consolidate open questions into one list, and define output artifacts. Do not produce any actual design solutions.
---

# Design Orchestrator (Agent-ready)

## Purpose
Create a deterministic design execution plan that coordinates multiple design-related skills.
This skill is the control plane: it plans **what to run**, **in what order**, **with what inputs**, and **how outputs will be merged**.

## When to use
Trigger when the user asks to:
- "設計を進めたい/設計方針をまとめたい"
- "どこから手をつけるべき？"
- "複数レイヤー（ドメイン/DB/UI/セキュリティ等）を横断して整理したい"
- "as-is と to-be のギャップを埋める計画を作りたい"
- "エージェントで並列に回したいので実行計画が欲しい"

## Non-goals (must NOT do)
- Do NOT propose actual design solutions (no schema options, no architecture diagrams, no UI wireframes).
- Do NOT refactor code or change files other than the plan artifact.
- Do NOT ask multiple rounds of questions. At most one consolidated question set in the plan.
- Do NOT run other skills directly (this skill plans; another runner executes).

## Inputs (use what is available)
- Problem statement / goal
- Current state availability (existing as-is docs or none)
- Constraints (time, tech stack, infra, compliance)
- Target scope: system/service/feature
- Known priorities (perf, security, speed, UX, cost)

If missing, proceed with best-effort and record as consolidated questions.

### Rule
Format:
DP-YYYYMMDD-<scope>-<slug>

- YYYYMMDD: current date (Asia/Tokyo)
- scope:
    - sys  = system-wide design
    - svc  = service-level design
    - feat = feature-level design
- slug:
    - kebab-case
    - 3–40 characters
    - alphanumeric and hyphens only
    - derived from normalized goal
    - avoid vague words (e.g., test, tmp, new)

### Behavior
- If user provides a design_id → use it as-is.
- If not provided → generate one using the rule above.
- If directory already exists → append incremental suffix:
    - -01, -02, -03, ...

Example:
DP-20260221-feat-president-listing
DP-20260221-feat-president-listing-01

## Artifact policy (1 Skill = 1 Artifact)
### Produces (must)
- `docs/design/<design_id>/design-plan.md`

The plan must be self-contained and deterministic.
If the file exists, overwrite it completely.

### Consumes (optional)
- `docs/as-is.md` (from as-is-discovery)
- existing ADRs / architecture docs / product requirements
- any prior design docs

## Core concept: Plan = DAG + Contracts
This orchestrator outputs:
1) a DAG (steps, dependencies, parallelism)
2) contracts for each step (input it needs + output it must produce)

## Required workflow (do not skip)
1. Normalize the goal into one sentence.
2. Set scope mode: `system-wide` or `service-level` or `feature-level`.
3. Determine whether `as-is-discovery` is required:
    - required if existing as-is is missing, stale, or scope is unclear
4. Select required design skills:
    - domain-design, db-design, app-design, architecture-design, ui-design, security-design
    - plus reviewers: design-review, security-review
5. Build an execution DAG:
    - mark which steps can be parallel
    - keep dependencies minimal but correct
6. Define artifact contracts:
    - each step produces exactly one file (suggested paths below)
7. Consolidate questions:
    - gather all missing inputs into a single list (with priority)
8. Write `docs/design/<design_id>/design-plan.md` in the strict format.


## Design ID Generation

The design-orchestrator is responsible for determining the `design_id`.

---

# STRICT OUTPUT FORMAT (docs/design/<design_id>/design-plan.md)

# Design Plan

## 1. Normalized Goal
- Goal (one sentence):
- Success criteria (3-7 bullets):
- Non-goals (optional):

## 2. Scope & Mode
- Mode: <system-wide|service-level|feature-level>
- In scope:
- Out of scope:
- Key assumptions (must label as assumptions):

## 3. Required Skills (Selected)
List selected skills and why they are needed:
- as-is-discovery: <needed/not needed> (reason)
- domain-design: ...
- db-design: ...
- app-design: ...
- architecture-design: ...
- ui-design: ...
- security-design: ...
- design-review: ...
- security-review: ...

## 4. Execution DAG
Represent the plan with explicit dependencies and parallelism.

### 4.1 DAG (Readable)
- Step 0: ...
    - Depends on: ...
    - Can run in parallel with: ...
- Step 1: ...

### 4.2 DAG (Machine-readable YAML)
```yaml
version: 1
mode: feature-level
steps:
  - id: as_is
    skill: as-is-discovery
    optional: true
    depends_on: []
    produces: docs/as-is.md
  - id: domain
    skill: domain-design
    optional: false
    depends_on: [as_is]
    produces: docs/design/<design_id>/domain-design.md
  - id: db
    skill: db-design
    optional: false
    depends_on: [domain]
    produces: docs/design/<design_id>/db-design.md
  - id: app
    skill: app-design
    optional: true
    depends_on: [domain, db]
    produces: docs/app-design.md
  - id: ui
    skill: ui-design
    optional: true
    depends_on: [domain]
    produces: docs/ui-design.md
  - id: arch
    skill: architecture-design
    optional: true
    depends_on: [as_is]
    produces: docs/architecture-design.md
  - id: sec
    skill: security-design
    optional: true
    depends_on: [as_is, arch]
    produces: docs/security-design.md
  - id: review
    skill: design-review
    optional: false
    depends_on: [domain, db, app, ui, arch]
    produces: docs/design/<design_id>/design-review.md
  - id: sec_review
    skill: security-review
    optional: true
    depends_on: [sec, review]
    produces: docs/security-review.md
```

## 5. Step Contracts (Inputs/Outputs)
For each step, define:
- Inputs required (artifacts, assumptions, questions)
- Output file (exact path)
- Definition of Done (checklist)

### 5.1 as-is-discovery
- Inputs:
- Produces: docs/as-is.md 
- DoD:
  - [ ] Static/Behavior/Constraints included 
  - [ ] Facts/Inferences labeled + confidence 
  - [ ] JSON summary included

### 5.2 domain-design
- Inputs:
- Produces: docs/design/<design_id>/domain-design.md 
- DoD:
  - [ ] Ubiquitous language glossary 
  - [ ] Aggregates + invariants 
  - [ ] Core use-cases mapping

(continue for all selected steps)

## 6. Consolidated Questions (ask user once)
List questions required to proceed, prioritized:
- (High) ...
- (Medium) ...
- (Low) ...

## 7. Output Artifacts (Final set)
- docs/design/<design_id>/design-plan.md (this file)
- docs/as-is.md (if executed)
- docs/design/<design_id>/domain-design.md 
- docs/design/<design_id>/db-design.md 
- docs/app-design.md (optional)
- docs/ui-design.md (optional)
- docs/architecture-design.md (optional)
- docs/security-design.md (optional)
- docs/design/<design_id>/design-review.md
- docs/security-review.md (optional)

## 8. Structured Summary (Machine-readable JSON)
```json
{
  "design_id": "DP-20260221-feat-president-listing",
  "goal": "",
  "mode": "feature-level",
  "selected_skills": [
    { "skill": "as-is-discovery", "required": true, "produces": "docs/as-is.md" }
  ],
  "dag": {
    "steps": [
      { "id": "as_is", "skill": "as-is-discovery", "depends_on": [], "produces": "docs/as-is.md" }
    ]
  },
  "questions": [
    { "text": "", "priority": "high|medium|low" }
  ],
  "final_artifacts": [
    "docs/design/<design_id>/design-plan.md",
    "docs/as-is.md"
  ]
}
```

## Quality bar / Checklist
- [ ] No actual design solutions included 
- [ ] Goal is one sentence and measurable success criteria exist 
- [ ] DAG includes dependencies and parallelism opportunities 
- [ ] Every step has a single output file path 
- [ ] Questions are consolidated into one list 
- [ ] JSON summary is valid JSON