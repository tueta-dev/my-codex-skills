# Modeling Checklist (Beginner)

## Conceptual
- Identify actors (users, admins, systems)
- Identify core objects (articles, orders, comments)
- Identify relationships (user writes article)

## Logical
- Use 1 table per responsibility
- Many-to-many => join table
- Prefer 3NF (no duplicated facts)
- Define primary keys and foreign keys

## Physical
- Pick clear data types
- Add unique constraints for identifiers (email, username)
- Add indexes for common queries
- Avoid NULL-heavy columns; split optional data

## Common Pitfalls
- Missing unique constraints (duplicates creep in)
- Storing derived data without refresh plan
- Over-indexing early
- Mixing multiple responsibilities in a single table
