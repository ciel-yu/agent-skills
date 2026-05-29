# Advanced Analysis for Boundary-Sensitive Migrations

Use this reference only when a module move or boundary change looks simple structurally but carries hidden semantic risk.

Do **not** turn the migration into a formal CFG/DFG exercise by default. Use control-flow and data-flow thinking only to explain caller-visible behavior that ordinary import mapping does not capture.

## When to Use

Reach for this reference when any of these are true:

- module initialization order affects exported values, registration, or side effects
- exceptions or retries inside module loading or dispatch change what callers observe
- a boundary move changes where derived values are computed, cached, or normalized
- dynamic loading, registries, dependency injection, or generated code hide the real path from consumer to implementation
- a supposedly pure re-export actually changes timing, side effects, or lifecycle behavior

## Practical Reading Model

- **Control-flow view**: trace load order, registration order, branch paths, exceptions, async handoff points, and teardown behavior that callers or runtime wiring depend on
- **Data-flow view**: trace how exported values, config, cached state, normalized inputs, and emitted outputs travel across the old and target boundaries

You usually only need a short written map. Formal graph construction is optional and rarely necessary.

## What to Capture

For each risky consumer path, note:

1. how the consumer discovers the module
2. what control path activates the module or export
3. what values cross the boundary and where they are transformed
4. what observable effect must remain stable: export shape, error behavior, side effect, ordering, or lifecycle
5. what temporary bridge is required, if any

## Migration Rule

Preserve what callers observe, not the exact internal path shape.

- If the same exported behavior can be preserved with a simpler boundary, simplify.
- If hidden path differences would change ordering, initialization, caching, or side effects, treat that as part of the semantic surface.
- If a path is accidental or dead, classify it before deciding whether to carry it through the migration.

## Output to Feed Back Into the Main Skill

Feed the result back into the main pipeline as:

- semantic-surface notes
- consumer/discovery map corrections
- migration-shape constraints
- surface-equivalence checks for risky cohorts
