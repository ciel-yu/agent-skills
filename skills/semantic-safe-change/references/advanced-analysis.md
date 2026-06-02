# Advanced Analysis

Use this reference only when ordinary interface inspection does not fully capture the semantic risk — when hidden control paths, value propagation, or lifecycle behavior could silently change across a refactor, boundary move, or technology migration.

Do **not** make CFG/DFG the default workflow. Use control-flow and data-flow thinking only to explain caller-visible behavior that straightforward analysis misses.

---

## When to use

Load this file when:

- branch order, exception paths, or retry logic affect externally observable results
- module initialization order affects exported values, registration, or side effects
- async sequencing, lifecycle transitions, or scheduler differences affect correctness
- a value is transformed through several intermediate steps before reaching an observable output
- dynamic loading, registries, dependency injection, or generated code hide the real path from consumer to implementation
- a supposedly pure re-export or adapter actually changes timing, side effects, or lifecycle behavior
- state-machine behavior is embedded in tangled conditionals rather than explicit types

---

## Analysis lenses

Use the concepts informally. A short written map is usually sufficient; formal graph construction is optional and rarely necessary.

- **Control-flow view**: trace load order, registration order, branch paths, exceptions, async handoff points, teardown behavior, and state transitions that callers or runtime wiring depend on.
- **Data-flow view**: trace how important values — exported values, config, cached state, normalized inputs, derived outputs — travel through the system, where they are transformed, cached, or dropped, and what crosses boundaries.

---

## Path inventory

For each risky path (refactor, boundary, or migration), note:

1. trigger condition
2. control path taken (including how consumers discover or activate it)
3. what values cross the boundary and where they are transformed
4. observable effect that must remain stable: output, side effect, ordering, lifecycle, error behavior
5. whether the behavior is Intentional, Relied-upon accidental, Ambiguous, or Dead
6. what temporary bridge is required, if any

---

## Rule

Preserve what callers observe, not the exact internal path shape.

- If the same observable behavior can be preserved with a simpler path, simplify.
- If hidden path differences would change ordering, initialization, caching, or side effects, treat that as part of the semantic surface.
- If a path is accidental or dead, classify it before deciding whether to carry it through.
- Keep branch structure only when callers observe it through behavior, ordering, timing, side effects, or error semantics.
- If two paths are structurally different but semantically equivalent, merge them in the replacement.

---

## Output

Feed the path inventory into:

- semantic contract or behavioral contract clauses
- legacy behavior classifications
- migration-shape constraints
- surface-equivalence checks for risky cohorts
- equivalence checks for sensitive paths
