# Semantic Contract

Use this reference to extract the semantics that must survive a refactor.

---

## What to capture

For the target module, flow, or subsystem, capture:

| Dimension | Questions |
|---|---|
| Domain meaning | What concept does this represent? Which canonical terms matter? |
| Inputs | What data enters? What shape, assumptions, defaults, and validation rules exist? |
| Outputs | What is returned, emitted, stored, or rendered? |
| Invariants | What must always remain true before and after execution? |
| Side effects | What is written, sent, logged, cached, or triggered? |
| Error behavior | How do failures surface? Which errors are retried, swallowed, transformed, or exposed? |
| Ordering | Does sequence matter? Are there transactional or lifecycle constraints? |
| Visibility | Which names or behaviors are public, shared-internal, or purely local? |
| Compatibility | Which old names, payload fields, or entry points still need support? |

---

## Preservation layers

State which layers must stay stable for this refactor:

1. **Behavior** - outputs, side effects, errors, ordering, retries
2. **Contract** - APIs, schemas, events, return shapes, caller expectations
3. **Domain meaning** - concepts, lifecycle stages, ownership, canonical terms
4. **Names** - public identifiers and semantically loaded labels

If a layer can change, say so explicitly and route the change through the ADR gate when needed.

---

## Sources of truth

Prefer evidence in this order:

1. explicit user constraints
2. tests and fixtures
3. external contracts: API docs, schemas, events, protocols
4. code at call sites and persistence boundaries
5. domain docs, ADRs, and specs
6. inline comments and naming
7. implementation details

If sources disagree, report the disagreement instead of guessing.

---

## Characterization checklist

Before replacement, try to answer:

- What behaviors are intentional vs accidental?
- Which quirks are relied on by callers?
- Which layers must remain stable: behavior, contract, domain meaning, names?
- Which names carry domain meaning and therefore must not drift casually?
- Which behaviors are user-visible, externally integrated, or persisted?
- Which changes would be safe only with an ADR?

---

## Minimal contract template

Use this shape in notes or planning output:

```md
## Semantic Contract
- Purpose:
- Detected intent + confidence:
- Preservation boundary:
- Canonical terms:
- Inputs:
- Outputs:
- Invariants:
- Side effects:
- Error behavior:
- Public/shared boundaries:
- Compatibility constraints:
- Known ambiguities:
```

A short explicit contract is better than an implicit rewrite plan.
