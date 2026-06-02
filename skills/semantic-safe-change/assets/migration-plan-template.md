# Semantic-Safe Module Boundary Migration Plan

Use this template for `audit` mode output. Fill every section; mark unknowns explicitly rather than leaving them blank.

---

```md
# Semantic-Safe Module Boundary Migration Plan: <module / boundary>

- Date:
- Author:
- Mode: audit | migrate

---

## Target

- Module / boundary:
- Current location:
- Target location or owning boundary:
- Rollout constraints:

---

## Semantic Surface

- Discovery paths (import paths, registry keys, DI tokens, config references):
- Exported entry points:
- Input / output contract:
- Error / failure contract:
- Side effects and lifecycle:
- Required invariants:

### Surface Classification

| Surface element | Class | Evidence | Notes |
|---|---|---|---|
| | Required / Relied-on incidental / Ambiguous / Dead | | |

---

## Consumer Map

### Cohort A — Internal direct callers

- Files / systems:
- Migration notes:

### Cohort B — Indirect or generated callers

- Files / systems:
- Migration notes:

### Cohort C — External or boundary callers

- Files / systems:
- Migration notes:

---

## Recommended Migration Shape

- Chosen shape:
- Why this is the smallest safe option:
- Temporary bridges needed (each must have a named owner and removal condition):

---

## Equivalence Plan

- Discovery checks:
- Shape checks:
- Behavior checks:
- Failure checks:
- Side-effect checks:
- Automation possible: Yes / Partial / No
- Manual coverage plan (if partial/no):

---

## Risk List

| Risk | Likelihood | Impact | Mitigation |
|---|---|---|---|
| | | | |

---

## Gate Notes

- Surface-break risks:
- Ambiguities requiring resolution:
- Required explicit decisions (if migration is not surface-preserving):

---

## Debt Carry-Forwards

Items that will remain after migration completes:

---

## Next Steps

1.
2.
3.
```
