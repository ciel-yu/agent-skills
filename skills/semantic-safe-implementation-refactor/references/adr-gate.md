# ADR Gate

Use this gate only when the refactor is no longer purely semantic-preserving.

---

## Open an ADR when the replacement intentionally changes

- public API names, schema fields, event names, or route shapes
- the meaning of a domain concept, state, or lifecycle stage
- consistency, durability, retry, or ordering guarantees in a user-visible or integration-visible way
- ownership boundaries between modules, services, or bounded contexts
- a long-lived architectural choice that future work will have to follow

If the change is purely internal and the semantic contract stays the same, do not force an ADR.

---

## Refactor-safe vs ADR-required

### Usually refactor-safe

- splitting one module into smaller private modules
- replacing internal algorithms without changing outputs or guarantees
- moving code behind a façade or adapter
- deleting dead branches that no caller can observe
- improving names that are purely local and not semantically loaded

### Usually ADR-required

- renaming public concepts or shared payload fields
- changing error semantics relied on by callers
- changing persistence or event semantics that affect downstream systems
- merging or splitting domain concepts
- turning a best-effort operation into a guaranteed one, or the reverse

---

## Minimal ADR shape for a refactor that changes semantics

```md
# ADR-XXXX: <title>

- Status: Proposed | Accepted
- Date: YYYY-MM-DD

## Context
## Existing Semantic Contract
## Proposed Change
## Options Considered
## Consequences
## Affected Artifacts
```

Keep the ADR focused on the semantic or architectural delta, not the mechanical code movement.
