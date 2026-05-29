# Intent Detection

Use this reference when the user's goal, the legacy code's purpose, or the preservation boundary is unclear.

---

## Intent types

Detect intent at three levels:

1. **User intent** - what the user wants changed, preserved, or avoided
2. **Artifact intent** - what the target module, service, handler, or flow is meant to do
3. **System intent** - what product or architecture constraints the wider system is enforcing

Do not collapse these into one guess.

---

## Evidence order

Infer intent from evidence in this order:

1. explicit user constraints
2. tests, fixtures, and snapshots
3. public contracts: APIs, schemas, events, protocols
4. call sites and downstream usage patterns
5. docs, ADRs, and specs
6. comments and naming
7. implementation details

If higher-priority evidence conflicts with lower-priority evidence, follow the higher-priority evidence and report the conflict.

---

## Extraction pass

For the request, pull out:

- **verbs** - rewrite, replace, refactor, optimize, migrate, extract
- **objects** - module, service, handler, API, subsystem, workflow
- **constraints** - do not change behavior, keep API stable, avoid hacks, preserve names, keep compatibility
- **non-goals** - what must not be optimized, redesigned, or removed

Often the real intent is in the constraints, not the verb.

---

## Preservation boundary

Before implementation, state which layers must remain stable:

- **Behavior** - outputs, side effects, errors, ordering
- **Contract** - APIs, schema fields, event names, caller expectations
- **Domain meaning** - concepts, lifecycle meaning, ownership boundaries, canonical terms
- **Names** - public identifiers and semantically loaded terms

If a layer is allowed to change, say so explicitly.

---

## Confidence model

Record intent with confidence:

- **High** - supported by tests or explicit user constraints
- **Medium** - supported by contracts, call sites, or docs
- **Low** - inferred mainly from names, comments, or implementation shape

Do not present low-confidence intent as settled fact.

---

## Ask instead of guessing

Stop and ask when:

- two preservation boundaries are both plausible
- tests and public contracts disagree
- a likely bug may also be relied-upon behavior
- the user asked for refactor language but the requested outcome implies product redesign
- domain terminology appears overloaded or ambiguous

A blocked question is safer than a semantic drift hidden inside a refactor.
