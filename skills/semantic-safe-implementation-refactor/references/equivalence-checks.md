# Equivalence Checks

Use this reference during `Verify Semantic Equivalence`.

Goal: prove the new path preserves the stated preservation boundary. Do not claim equivalence from code inspection alone.

---

## 1. Match evidence to the preserved layers

For each layer marked as stable, gather evidence that directly checks it.

| Preserved layer | What to compare | Strong evidence |
|---|---|---|
| Behavior | outputs, errors, side effects, ordering, retries | existing tests, characterization tests, fixture diffs, side-effect assertions |
| Contract | API shape, schema fields, event names, payloads, return values | contract tests, schema validation, API/event transcript diffs |
| Domain meaning | state transitions, lifecycle meaning, invariants, canonical terms | scenario tests, invariant checks, persistence assertions, business-flow comparisons |
| Names | public identifiers, semantically loaded labels | surface audit, payload/schema diff, route/event/name checks |

If a layer is marked stable but has no evidence, equivalence is not proven yet.

---

## 2. Prefer evidence in this order

1. existing tests that already encode the contract
2. new characterization tests for legacy behavior
3. fixture-based before/after comparisons
4. API, event, or CLI transcript diffs
5. snapshot or golden-output comparisons
6. structured manual scenario matrix

Use more than one method when blast radius is high.

---

## 3. Common equivalence patterns

### A. Characterization tests

Use when the legacy path has behavior but weak docs.

- capture current outputs for representative and edge-case inputs
- assert current error behavior, not idealized behavior
- cover quirks only if callers rely on them

### B. Fixture-based before/after comparison

Use when the same inputs can be run through old and new paths.

- record input fixtures
- capture outputs, side effects, emitted events, and persisted state
- diff old vs new results
- explain any approved differences explicitly

### C. Transcript comparison

Use for APIs, events, queues, and CLIs.

Compare:

- status codes / exit codes
- payload shapes and field semantics
- event names and ordering
- headers, metadata, or retry markers when relevant

### D. Side-effect assertions

Use when semantics live in writes, logs, or downstream triggers.

Check:

- database writes and state transitions
- emitted events / messages
- cache invalidation
- audit logs
- external calls and ordering

### E. Scenario matrix

Use when automation is incomplete.

At minimum, cover:

- happy path
- boundary conditions
- known edge cases
- representative failure modes
- compatibility cases using legacy names or payloads

---

## 4. What not to preserve blindly

Do not preserve a behavior just because it exists in the code.

Flag instead of silently preserving when a behavior looks like:

- a likely bug
- dead code with no observable caller
- inconsistent behavior across call sites
- an accidental quirk with no evidence of reliance

If you are unsure whether a quirk is relied on, mark it as ambiguous and ask or report it.

---

## 5. Acceptance bar

Before cutover, the evidence should show:

- each preserved layer has at least one concrete check
- known ambiguities are listed, not hidden
- approved semantic differences are explicit and justified
- high-blast-radius changes have more than one evidence source
- manual-only evidence says why automation was not practical

If these are not true, say "equivalence not yet proven" rather than overclaiming.

---

## 6. Minimal evidence summary template

Use this in audit notes or final delivery:

```md
## Equivalence Evidence
- Preserved layers:
- Evidence used:
- Legacy path compared:
- New path compared:
- Approved differences:
- Remaining ambiguities:
- Confidence: High | Medium | Low
```
