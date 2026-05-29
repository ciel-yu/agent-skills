# Surface Equivalence Checks

Use during `Verify Surface Equivalence`.

Goal: prove the target boundary still delivers what callers relied on through the old boundary. Code inspection alone is not equivalence evidence.

---

## 1. Match evidence to the preserved surface

For each surface element classified as Required or Relied-on incidental, gather evidence that directly checks it.

| Surface element | What to compare | Strong evidence |
|---|---|---|
| Discovery | old and new import paths resolve as expected | import resolution test, module resolution check |
| Shape | exported names, signatures, types, defaults, nullability | type check, contract test, shape diff |
| Behavior | outputs, return values, mutations for representative callers | existing tests, characterization tests, driver scripts |
| Failure | error types, messages, nullability, retryability | error-path tests, API transcript |
| Side effects | writes, events, caching, initialization, cleanup | side-effect assertions, before/after fixture comparison |

If a Required surface element has no evidence, equivalence is not yet proven.

---

## 2. Evidence preference order

1. existing tests that pin the surface
2. new migration tests at boundary entry points
3. type or schema validation checks
4. fixture-based before/after comparisons
5. minimal driver scripts that import and call representative flows
6. runtime smoke checks through the real surface

Use more than one method when migration scope is large or blast radius is high.

---

## 3. Common equivalence patterns

### A. Import resolution check

Confirm old and new discovery paths resolve to the right entry point:

- import through both the new path and any preserved compatibility re-export
- assert exported symbols and their types match the preserved contract

### B. Characterization tests

Use when the legacy module has behavior but weak test coverage.

- capture current outputs for representative inputs before migration
- assert the same outputs through the new boundary
- cover quirks only if callers demonstrably rely on them

### C. Fixture-based before/after comparison

Use when the same inputs can run through both the old and new boundary.

- record input fixtures
- capture outputs, side effects, emitted events
- diff old vs new results
- explain any approved differences explicitly

### D. Side-effect assertions

Use when semantics live in writes, events, or lifecycle behavior.

Check:

- database or state writes
- emitted events or messages
- initialization and teardown sequence
- cache behavior or singleton assumptions

---

## 4. What not to preserve blindly

Do not preserve a surface element just because it exists in the old module.

Flag instead of silently preserving when a behavior looks like:

- a likely bug or accident with no evidence of caller reliance
- dead code with no observable consumers
- an inconsistent export that conflicts across discovery paths

If you are unsure whether a quirk is relied on, mark it Ambiguous and ask or report it.

---

## 5. Acceptance bar

Before cutover, evidence must show:

- each Required surface element has at least one concrete check
- Relied-on incidental elements are either preserved or explicitly negotiated
- known ambiguities are listed, not hidden
- approved surface differences are explicit and justified
- manual-only evidence states why automation was not practical

State "equivalence not yet proven" rather than claiming it if these conditions are not met.

---

## 6. Evidence summary template

```md
## Equivalence Evidence
- Preserved surface elements:
- Evidence used:
- Old boundary checked:
- New boundary checked:
- Approved differences:
- Remaining ambiguities:
- Confidence: High | Medium | Low
```
