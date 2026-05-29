# Equivalence Checks

Prove that behavioral semantics are preserved after the surface transformation. Surface change does not imply behavioral equivalence; evidence must be explicit.

---

## Goal

For each migrated cohort, show that:

- outputs are the same for the same inputs
- side effects occur at the same points with the same semantics
- error behavior is unchanged from the caller's perspective
- ordering, lifecycle, and initialization behave equivalently
- user-visible behavior is unchanged

---

## Evidence methods

| Method | When to use | Strength |
|---|---|---|
| Existing tests pass without modification | Tests encode behavior, not tech-specific API syntax | High |
| New characterization tests against the old path | Legacy path has behavior but weak test coverage | High |
| Fixture-based before/after comparison | Same inputs can run through old and new tech | High |
| E2E or integration test suite | User-scenario level verification | High |
| API / event / CLI transcript diff | HTTP APIs, event-driven systems, CLI tools | Medium |
| Manual scenario matrix | When automation is incomplete | Low — document why automation was not practical |

Use more than one method when blast radius is high.

---

## What to verify per behavioral contract dimension

| Dimension | What to check |
|---|---|
| Domain purpose | Same output for representative and edge-case inputs |
| Invariants | Invariants hold in the new implementation |
| Side effects | Writes, emits, caches, and external calls occur at the same points |
| Error behavior | Same errors surface to callers; retry/swallow/transform logic unchanged |
| Ordering / sequencing | Sequence constraints preserved; async behavior equivalent |
| Performance / lifecycle | Initialization, teardown, and any relied-on timing characteristics |

---

## What is NOT required

In a tech migration you are **not** required to preserve:

- specific method names, import paths, or call syntax (surface — intentionally changes)
- internal implementation details not observable by callers
- deprecated patterns being intentionally retired

Verify the behavioral contract, not surface similarity.

---

## Common patterns

### Characterization tests

Use when the legacy path has behavior but weak docs or test coverage.

- Capture current outputs for representative and edge-case inputs before migrating.
- Assert current error behavior, not idealized behavior.
- Cover quirks only if callers demonstrably rely on them.

### Fixture-based before/after comparison

Use when the same inputs can run through both old and new tech.

- Record input fixtures against the old implementation.
- Capture outputs, side effects, emitted events, and persisted state.
- Diff old vs new results after migration.
- Explain any approved differences explicitly.

### Scenario matrix

Use when automation is incomplete or the tech does not support parallel execution.

Cover at minimum:

- happy path
- boundary conditions
- known edge cases
- representative failure modes
- any compatibility cases inherited from the behavioral contract

---

## Acceptance bar

Before cutover, evidence must show:

- each behavioral contract dimension has at least one concrete check
- known ambiguities are listed, not hidden
- approved surface differences are explicit and justified
- high-blast-radius cohorts have more than one evidence source
- manual-only evidence states why automation was not practical

If these conditions are not met, state "equivalence not yet proven" rather than claiming it.

---

## Evidence summary template

```md
## Equivalence Evidence
- Cohort:
- Behavioral contract items verified:
- Evidence methods used:
- Approved surface-only differences:
- Remaining ambiguities:
- Confidence: High | Medium | Low
```
