# Design Review Checklist

## Completeness
- All critical flows covered (happy + failure paths)
- Assumptions are explicit
- Open questions are prioritized

## Consistency
- Terms match glossary
- IDs/keys align between domain and DB
- Invariants have enforcement strategy

## Implementability
- Responsibilities per layer are clear
- No circular dependencies in plan
- Artifacts define next steps

## Risk
- Security: authz per use-case, audit logging needs
- Performance: key queries identified with indexes
- Ops: deploy/rollback constraints, observability