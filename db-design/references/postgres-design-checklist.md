# PostgreSQL DB Design Checklist

## Modeling
- Entities map cleanly from domain aggregates
- No invariant requires cross-aggregate transaction unless intentional
- Avoid duplication; document any denormalization

## Constraints
- PK/FK/UNIQUE/CHECK/NOT NULL are explicit
- FK delete/update rules are intentional and documented

## Indexes
- Indexes exist to support explicit queries (avoid speculative indexes)
- Composite index column order matches filters/sorts
- Consider partial indexes only with clear predicates

## Operations
- Migration path considered if as-is exists
- Avoid "soft delete everywhere" unless explicitly required