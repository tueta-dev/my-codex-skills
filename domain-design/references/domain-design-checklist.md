
# Domain Design Checklist

## Glossary
- Terms are consistent and not overloaded
- Synonyms/anti-terms captured

## Contexts
- Each context has one primary responsibility
- Context map relationships are stated clearly

## Aggregates
- Invariants are explicit
- Aggregate boundaries prevent invariants from spanning multiple aggregates
- Commands are cohesive; avoid "god aggregate"

## Events
- Events represent meaningful business facts (past tense)
- Event triggers are explicit
- Consumers are domain-level (not technical)

## Use-cases
- Preconditions/postconditions are clear
- Failure cases include validation and business rule failures