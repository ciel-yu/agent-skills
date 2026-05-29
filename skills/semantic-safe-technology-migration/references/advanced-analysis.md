# Advanced Analysis for Behavior-Sensitive Technology Migrations

Use this reference only when a technology migration changes APIs mechanically but the real risk lives in hidden behavior.

Do **not** make CFG or DFG the default frame. Use control-flow and data-flow analysis only when they help explain behavior that must survive across framework, runtime, module-system, or programming-model change.

## When to Use

Reach for this reference when any of these are true:

- async scheduling, batching, or event-loop differences can change outcomes
- lifecycle timing or teardown order differs across source and target tech
- exception propagation, cancellation, retries, or fallback paths differ
- values are transformed through compatibility layers before reaching observable outputs
- official migration guides describe surface changes but leave behavior-sensitive gaps

## Practical Reading Model

- **Control-flow view**: trace lifecycle hooks, async boundaries, exceptions, retries, cancellation, scheduling points, and teardown order that affect behavior
- **Data-flow view**: trace how inputs, transformed values, cached state, emitted events, and persisted data move through old and new stacks

Use lightweight path inventories unless a formal graph is truly needed.

## What to Capture

For each risky migration path, write down:

1. source-tech path and target-tech path
2. trigger or lifecycle phase
3. critical values entering, transforming, and leaving the path
4. observable behavior that must remain equivalent
5. whether the difference is Surface-Only, Behavioral, or Ambiguous

## Migration Rule

Preserve behavioral meaning, not technical shape.

- A new framework lifecycle is acceptable if the same domain behavior survives.
- A different async model is acceptable only if ordering, visibility, error semantics, and side effects that matter still hold.
- If control or data-path differences create Behavioral or Ambiguous risk, stop at the gate rather than silently migrating through it.

## Output to Feed Back Into the Main Skill

Feed the result back into the main pipeline as:

- behavioral-contract clauses
- breaking-change classifications
- compatibility-layer constraints
- behavioral-equivalence checks for risky cohorts
