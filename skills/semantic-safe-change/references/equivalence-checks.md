# Equivalence Checks

Prove that behavior survives the change — whether the change is a refactor, boundary move, or technology migration. Code inspection alone is not equivalence evidence.

---

## 1. Match evidence to what is being preserved

For each element classified as Required, Relied-on incidental, or part of the behavioral contract, gather evidence that directly checks it.

| What to preserve | What to compare | Strong evidence |
|---|---|---|
| Behavior | outputs, errors, side effects, ordering, retries | existing tests, characterization tests, fixture diffs, side-effect assertions |
| Contract / Shape | API shape, schema fields, event names, return values, signatures, types | contract tests, schema validation, type checks, API/event transcript diffs |
| Domain meaning / purpose | state transitions, lifecycle meaning, invariants, canonical terms | scenario tests, invariant checks, persistence assertions, business-flow comparisons |
| Names / Discovery | import paths, exported names, discovery routes | surface audit, import resolution tests, payload/schema diff |
| Side effects | writes, events, caching, initialization, cleanup, external calls | side-effect assertions, before/after fixture comparison |
| Failure | error types, messages, nullability, retryability | error-path tests, API transcript |

If a preserved element has no evidence, equivalence is not yet proven.

---

## 2. Evidence preference order

1. existing tests that already encode the contract or behavior
2. new characterization tests for legacy behavior
3. type or schema validation checks
4. fixture-based before/after comparisons
5. API, event, or CLI transcript diffs
6. snapshot or golden-output comparisons
7. minimal driver scripts or runtime smoke checks through the real surface
8. structured manual scenario matrix

Use more than one method when blast radius is high or migration scope is large.

---

## 3. Common equivalence patterns

### A. Import resolution check

Confirm old and new discovery paths resolve to the right entry point:

- import through both the new path and any preserved compatibility re-export
- assert exported symbols and their types match the preserved contract

### B. Characterization tests

Use when the legacy path has behavior but weak docs or test coverage.

- capture current outputs for representative and edge-case inputs before changing
- assert current error behavior, not idealized behavior
- cover quirks only if callers demonstrably rely on them

### C. Fixture-based before/after comparison

Use when the same inputs can run through old and new paths.

- record input fixtures
- capture outputs, side effects, emitted events, and persisted state
- diff old vs new results
- explain any approved differences explicitly

### D. Transcript comparison

Use for APIs, events, queues, and CLIs.

Compare status codes, payload shapes and field semantics, event names and ordering, headers, metadata, or retry markers.

### E. Side-effect assertions

Use when semantics live in writes, events, or lifecycle behavior.

Check database writes, state transitions, emitted events, cache behavior, audit logs, external calls and ordering, initialization and teardown sequence.

### F. Scenario matrix

Use when automation is incomplete.

Cover at minimum: happy path, boundary conditions, known edge cases, representative failure modes, and any compatibility cases inherited from the contract.

---

## 4. What not to preserve blindly

Do not preserve a behavior or surface element just because it exists in the old code.

Flag instead of silently preserving when it looks like:

- a likely bug or accident with no evidence of caller reliance
- dead code with no observable consumers
- an inconsistent export that conflicts across discovery paths
- an accidental quirk with no evidence of reliance
- deprecated patterns being intentionally retired (technology migration)

If unsure whether a quirk is relied on, mark it Ambiguous and ask or report it.

---

## 5. Acceptance bar

Before cutover, evidence must show:

- each preserved element has at least one concrete check
- Relied-on incidental elements are either preserved or explicitly negotiated
- approved differences (semantic or surface) are explicit and justified
- high-blast-radius changes have more than one evidence source
- manual-only evidence states why automation was not practical

State "equivalence not yet proven" rather than claiming it if these conditions are not met.

---

## 6. Evidence summary template

```md
## Equivalence Evidence
- Preserved elements:
- Evidence used:
- Old path checked:
- New path checked:
- Approved differences:
- Cohort (if applicable):
```
