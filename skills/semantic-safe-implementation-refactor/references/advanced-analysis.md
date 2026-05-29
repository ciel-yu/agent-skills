# Advanced Analysis for Control-Sensitive or Data-Sensitive Refactors

Use this reference only when the legacy implementation is hard to preserve semantically by reading interfaces and tests alone.

Do **not** make CFG or DFG the main workflow. They are optional analysis lenses that help extract the semantic contract when observable behavior depends on hidden path structure.

## When to Use

Reach for this reference when any of these are true:

- branch order changes externally observable results
- exception or retry paths affect output, logging, or side effects
- async sequencing or lifecycle transitions affect correctness
- a value is transformed through several intermediate steps before it reaches an externally observable output
- state-machine behavior is embedded in tangled conditionals rather than explicit types

## Practical Reading Model

Use the concepts informally:

- **Control-flow view**: trace the branches, early returns, exceptions, retries, and state transitions that lead to different observable outcomes
- **Data-flow view**: trace how important values are derived, normalized, cached, mutated, or dropped before reaching outputs, storage, events, or side effects

You do **not** need to build a formal compiler-grade graph. A written path inventory is usually enough.

## What to Capture

For each risky path, write down:

1. trigger condition
2. control path taken
3. important values entering and leaving the path
4. observable result: returned value, emitted event, stored data, error, timing, or side effect
5. whether the behavior is Intentional, Relied-upon accidental, Ambiguous, or Dead

## Refactor Rule

Preserve the **meaning** of the path, not its exact structure.

- Keep branch structure only when callers observe it through behavior, ordering, timing, side effects, or error semantics.
- If two legacy paths are structurally different but semantically equivalent, merge them in the replacement.
- If the legacy structure contains dead or accidental behavior, classify it before deciding whether to preserve it.

## Output to Feed Back Into the Main Skill

Feed the result back into the main pipeline as:

- semantic contract clauses
- legacy behavior classifications
- equivalence checks for sensitive paths
- seam constraints for the replacement design
