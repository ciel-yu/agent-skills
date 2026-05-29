# Tech Migration Plan

Use this template for `audit` mode output. Fill every section; mark unknowns explicitly rather than leaving them blank.

---

```md
# Tech Migration Plan: <target scope>

- Date:
- Author:
- Source tech / version:
- Target tech / version:
- Scope:
- Mode: audit
- Rollout constraints:

---

## Behavioral Contract

What must be preserved regardless of surface change:

- Domain purpose:
- Inputs:
- Outputs:
- Invariants:
- Side effects:
- Error behavior:
- Ordering / lifecycle guarantees:
- Performance / SLA constraints:
- User-visible behavior:
- Known ambiguities:

---

## Breaking Change Inventory

| Change | Source | Class | Affected consumers | Automation | Decision / action |
|---|---|---|---|---|---|
| | | Surface-Only / Behavioral / Ambiguous | | | |

Behavioral-Risk Gate items — decisions required before migration proceeds:

1.
2.

---

## Consumer Map

- Total consumers:
- Cohort A (isolated internal): N — [location or search command]
- Cohort B (shared internal): N — [location or search command]
- Cohort C (generated / indirect): N — [location or search command]
- Cohort D (boundary / external): N — [location or search command]
- Automation available: Yes / Partial / No
- Tooling: [codemod name, lint rule, etc.]

---

## Recommended Migration Strategy

- Strategy:
- Why this strategy:
- Codemod tools identified:
- Manual work required:

---

## Compatibility Layer Plan

| Bridge | Type | Owner | Removal condition | Expected lifetime | Remaining callers |
|---|---|---|---|---|---|
| | | | | | |

---

## Cohort Migration Plan

### Cohort A — Isolated internal
- Files / modules:
- Migration approach:
- Verification method:

### Cohort B — Shared internal
- Files / modules:
- Migration approach:
- Verification method:

### Cohort C — Generated / indirect
- Files / modules:
- Migration approach:
- Verification method:

### Cohort D — Boundary / external
- Files / modules:
- Migration approach:
- Verification method:

---

## Equivalence Plan

- Behavioral contract items to verify:
- Evidence methods:
- Automation possible: Yes / Partial / No
- Manual coverage plan (if partial/no):

---

## Risk List

| Risk | Likelihood | Impact | Mitigation |
|---|---|---|---|
| | | | |

---

## Debt Carry-Forwards

Items that will remain after migration completes:

---

## Next Steps

1.
2.
3.
```
