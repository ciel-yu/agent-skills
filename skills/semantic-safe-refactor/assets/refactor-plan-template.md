# Refactor Plan

Use this template for `audit` mode output. Fill every section; mark unknowns explicitly rather than leaving them blank.

---

```md
# Refactor Plan: <target module / service / subsystem>

- Date:
- Author:
- Mode: audit | replace

---

## Target Surface
- Files / modules / services:
- Entry points:
- Downstream consumers:

---

## Detected Intent
- User intent:
- Artifact intent:
- System intent:
- Confidence: High | Medium | Low
- Evidence sources:

---

## Preservation Boundary
Layers that must remain stable:
- [ ] Behavior (outputs, side effects, errors, ordering)
- [ ] Contract (APIs, schemas, events, return shapes)
- [ ] Domain meaning (concepts, lifecycle, canonical terms)
- [ ] Names (public identifiers, semantically loaded labels)

Layers allowed to change (state explicitly):

---

## Legacy Behavior Inventory
| Behavior | Class | Evidence | Action | Notes |
|---|---|---|---|---|
| | Intentional / Relied-upon accidental / Ambiguous / Dead | | Preserve / Flag / Drop | |

---

## Semantic Contract
- Purpose:
- Detected intent + confidence:
- Preservation boundary:
- Canonical terms:
- Inputs:
- Outputs:
- Invariants:
- Side effects:
- Error behavior:
- Public/shared boundaries:
- Compatibility constraints:
- Known ambiguities:

---

## Replacement Seam
- Chosen seam:
- Why this seam:
- Boundary introduced or reused:
- Caller impact:
- Rollback path:
- Equivalence check point:
- Residual legacy touchpoints:

---

## Equivalence Plan
- Preserved layers to check:
- Evidence methods:
- Automation possible: Yes | Partial | No
- If partial/no, manual coverage plan:
- Remaining ambiguities:

---

## ADR Requirement
- Required: Yes | No
- If yes, reason:
- If yes, draft title:

---

## Risk List
| Risk | Likelihood | Impact | Mitigation |
|---|---|---|---|
| | | | |

---

## Debt Carry-Forwards
Items that cannot be cleaned in this refactor:

---

## Next Steps
1.
2.
3.
```
