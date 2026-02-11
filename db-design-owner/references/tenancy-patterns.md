# Multi-Tenant Patterns (PostgreSQL)

## 1) Shared Schema + tenant_id (default)
- **Shape**: All tenant data in the same tables, each row has tenant_id.
- **Pros**: Simpler ops, easy to query across tenants, cheaper infra.
- **Cons**: Stronger need for app-level isolation, noisy-neighbor risk, harder per-tenant customization.
- **Use when**: Early-stage SaaS, moderate scale, need cross-tenant analytics.

## 2) Schema per Tenant
- **Shape**: One PostgreSQL schema per tenant, same table set per schema.
- **Pros**: Stronger isolation, per-tenant migrations possible.
- **Cons**: Operational complexity, cross-tenant analytics harder.
- **Use when**: Strict isolation, enterprise tenants, customizations per tenant.

## 3) Database per Tenant
- **Shape**: One database per tenant (or cluster by tenant tiers).
- **Pros**: Max isolation, easier per-tenant restore.
- **Cons**: High ops cost, connection management complexity.
- **Use when**: Very large tenants or regulatory isolation needs.

## RLS Note (optional)
- PostgreSQL Row Level Security can enforce tenant isolation for shared schema.
- Use when security requirements demand DB-level isolation in shared schema.
