---
name: as-is-discovery
description: Use when asked to understand an existing system (as-is). Observe current structure, runtime behavior, and constraints; clearly separate facts vs inferences with confidence; produce a strict Markdown report plus a machine-readable JSON summary for orchestration. Do not propose any to-be design.
---

# As-Is Discovery (Agent-ready v2)

## Purpose
Produce an "as-is" understanding of an existing system in a way that supports:
- parallel agent execution
- deterministic merging by an orchestrator
- downstream review skills (design/security)

This skill must:
- observe (not design)
- label evidence and confidence
- output both human-readable and machine-readable artifacts

## When to use
Use this skill when the user asks to:
- understand the current data structure / schema
- understand current architecture / services / dependencies
- summarize request flows / transactions / jobs
- identify constraints, implicit assumptions, and unknowns

## Non-goals (must NOT do)
- Do NOT propose a to-be architecture, refactor plan, or redesign.
- Do NOT prescribe technologies or patterns.
- Do NOT provide optimization solutions (perf/security/etc).
- If the user explicitly requests proposals, stop and suggest switching to a design skill.

## Discovery Mode (must set)
Choose exactly one mode. If the user doesn't specify, default to `service-level`.

- `system-wide`: broad overview across repos/services
- `service-level`: deep dive for one service/repo
- `feature-level`: deep dive for one user journey / endpoint / feature

## Scope Inputs (ask only if missing AND blocking)
Ask at most one message with the minimum needed:
- Mode: system-wide / service-level / feature-level
- Target repo/service (if not obvious)
- Focus journey/endpoints (if feature-level)
- Environment relevance (dev/prod) if constraints differ

If still unknown, proceed with best-effort and record as Open Questions.

## Artifacts (for orchestration)
### Produces
- `docs/as-is/as-is-YYYY-MM-DD.md`

The file must contain:
1. The full Markdown As-Is Discovery Report (strict format).
2. A final section titled "Structured Summary (Machine-readable JSON)" containing a valid JSON block that follows the defined schema.

If the file already exists, overwrite it completely.
Do not append partial reports.
The output must be deterministic and self-contained.

### Consumes (optional)
- existing docs (README, ADRs, architecture docs)
- configs (env examples, infra configs)
- code evidence (migrations, routes, controllers, services)

## Evidence policy
- Prefer code/config/docs as evidence over assumptions.
- Every Fact must cite an evidence item (a file/config/doc reference) or explicitly say "Evidence: not available".
- Every Inference must include:
    - Reason (why inferred)
    - Confidence (low/medium/high)
- If evidence is insufficient: record an Open Question instead of guessing.

## Required workflow (do not skip)
1. Set `Discovery Mode` and define scope.
2. Collect evidence (only what is available).
3. Extract Facts (evidence-backed).
4. Extract Inferences (with reason + confidence).
5. Extract Constraints and Implicit Assumptions.
6. List Open Questions.
7. Output:
    - Markdown report (strict format)
    - JSON summary (strict schema)

---

# STRICT OUTPUT FORMAT (must follow)

# As-Is Discovery Report (v2)

## 0. Mode & Scope
- Discovery mode: <system-wide|service-level|feature-level>
- In scope:
- Out of scope:
- Evidence sources:
    - E1: <source> (type: code|config|doc|log|other)
    - E2: ...

## 1. Static Structure (Static)
### 1.1 Repositories / Modules
- Facts:
    - <fact> (Evidence: E1)
- Inferences:
    - <inference> (Reason: <...>, Confidence: low|medium|high)

### 1.2 Service Boundaries
- Facts:
- Inferences:

### 1.3 Data Model Summary (DB / Schema)
- Facts:
- Inferences:
- Key entities/tables:
    - <name>: <role/notes>

### 1.4 External Integrations
- Facts:
- Inferences:

## 2. Runtime Behavior (Behavior)
### 2.1 Key Request / Use-case Flows
- Flow A: <name>
    - Facts:
    - Inferences:
- Flow B: ...

### 2.2 Transaction Boundaries
- Facts:
- Inferences:

### 2.3 Async / Events / Jobs
- Facts:
- Inferences:

### 2.4 Error Handling / Observability
- Facts:
- Inferences:

## 3. Constraints (Constraints)
### 3.1 Technical Constraints
- Facts:
- Inferences:

### 3.2 Non-functional Constraints (perf/availability/scale)
- Facts:
- Inferences:

### 3.3 Security Constraints (authn/authz/data)
- Facts:
- Inferences:

### 3.4 Operational Constraints (deploy/CI/CD/ops)
- Facts:
- Inferences:

### 3.5 Implicit Assumptions (must be labeled inference)
- Assumptions:
    - <assumption> (Reason: <...>, Confidence: low|medium|high)

## 4. Open Questions (must include)
- Q1:
- Q2:

## 5. Inconsistency Signals (optional)
List observed smells without suggesting fixes.
- Signal:
    - Evidence: E?
    - Risk (hypothesis):
    - Confidence: low|medium|high

## 6. Structured Summary (Machine-readable JSON) (must include)
```json
{
  "design_id": "NA",
  "artifact": "as-is-discovery",
  "source_refs": {
    "as_is": "docs/as-is/as-is-YYYY-MM-DD.md"
  },
  "mode": "service-level",
  "scope": {
    "in_scope": [],
    "out_of_scope": []
  },
  "evidence": [
    { "id": "E1", "type": "code|config|doc|log|other", "ref": "path-or-description" }
  ],
  "static": {
    "repos_modules": {
      "facts": [ { "text": "", "evidence_ids": ["E1"] } ],
      "inferences": [ { "text": "", "reason": "", "confidence": "low|medium|high" } ]
    },
    "service_boundaries": {
      "facts": [],
      "inferences": []
    },
    "data_model": {
      "key_entities": [ { "name": "", "role": "", "notes": "" } ],
      "facts": [],
      "inferences": []
    },
    "external_integrations": {
      "facts": [],
      "inferences": []
    }
  },
  "behavior": {
    "flows": [
      {
        "name": "",
        "facts": [ { "text": "", "evidence_ids": ["E1"] } ],
        "inferences": [ { "text": "", "reason": "", "confidence": "low|medium|high" } ]
      }
    ],
    "transactions": { "facts": [], "inferences": [] },
    "async_events_jobs": { "facts": [], "inferences": [] },
    "observability": { "facts": [], "inferences": [] }
  },
  "constraints": {
    "technical": { "facts": [], "inferences": [] },
    "non_functional": { "facts": [], "inferences": [] },
    "security": { "facts": [], "inferences": [] },
    "operational": { "facts": [], "inferences": [] },
    "implicit_assumptions": [
      { "text": "", "reason": "", "confidence": "low|medium|high" }
    ]
  },
  "open_questions": [
    { "text": "", "priority": "high|medium|low" }
  ],
  "inconsistency_signals": [
    { "text": "", "evidence_ids": ["E1"], "risk_hypothesis": "", "confidence": "low|medium|high" }
  ]
}
```

## Quality bar / Checklist (must satisfy)
- [ ] No to-be proposals included
- [ ] Facts and inferences are explicitly separated
- [ ] Every inference has reason + confidence
- [ ] Static/Behavior/Constraints are all present
- [ ] Open Questions present (even if empty, state "None identified")
- [ ] Evidence list exists (even if small) and Facts reference evidence ids when possible
- [ ] JSON summary is valid JSON and matches the schema shape above
- [ ] Uses concise bullet points; avoids long prose
- [ ] Concise bullets; avoid long prose
