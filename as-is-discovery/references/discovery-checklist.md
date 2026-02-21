# Discovery Checklist

## Static
- Repo/monorepo? package boundaries?
- Frameworks, runtime, versions
- DB type, migrations, naming conventions
- Key tables + relationships
- External services (Auth, payment, analytics, storage)

## Behavior
- Request flow entrypoints (routes/controllers)
- Use-case/service layer presence
- Transaction boundaries and retry behavior
- Background jobs / queues / events
- Error handling patterns and logging

## Constraints
- Deployment topology, CI/CD
- Env vars / secrets management
- SLO/SLI if any
- Security model (authn/authz, roles/scopes)
- Performance hotspots (if evidence exists)