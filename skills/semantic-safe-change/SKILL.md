---
name: semantic-safe-change
description: "Use when code must change while observable behavior is preserved: rewriting a service behind an unchanged interface, moving a module to a new package or path, upgrading a framework or runtime, switching technology stacks, or any refactor where existing semantics must survive. Handles implementation refactors, boundary migrations, and technology migrations. Do not use for tiny local fixes, style-only cleanup, greenfield work, or changes with no existing behavioral baseline."
---

# Semantic-Safe Change

Make code changes that preserve behavioral semantics — whether replacing an implementation, moving a module boundary, or migrating technology. This skill is a **Pipeline + Reviewer** that selects the correct variant based on what is changing.

---

## Use When

Use when the user asks to:

- rewrite or replace an implementation while keeping the public surface stable
- move a module to a new package, path, or ownership boundary without breaking callers
- upgrade a framework, runtime, library version, or switch technology while preserving behavior
- extract, consolidate, or restructure code where behavior must survive the change

Do **not** use for:

- tiny local fixes where change discipline is unnecessary
- style-only cleanup, formatting, or mechanical renames with no behavioral risk
- greenfield work with no existing behavioral baseline
- redesigns that intentionally change product behavior without an explicit decision

---

## Boundary Test

Select the variant by answering: **what is the primary thing changing?**

| Primary change | Variant | Section link |
|---|---|---|
| Implementation internals, keeping the same interface, boundary, and technology | **Implementation Refactor** | [Variant A](#variant-a-implementation-refactor) |
| Module, package, or path boundary, keeping the caller-visible surface stable | **Boundary Migration** | [Variant B](#variant-b-boundary-migration) |
| Framework, runtime, version, module system, or programming model | **Technology Migration** | [Variant C](#variant-c-technology-migration) |

If two things are changing at once, select the variant for the **primary source of risk**:
- Technology change + implementation rewrite → Technology Migration (the tech change drives the plan).
- Boundary move + implementation rewrite → choose by which is riskier for callers.
- Technology change + boundary move → Technology Migration (the tech surface change subsumes the boundary move).

When in doubt, ask the user which concern is primary.

---

## Operating Modes

- **`audit`** — analyze the current state, map risks, and produce a change plan without editing files.
- **`execute`** — implement the change, add bridging code, migrate cohorts, prove equivalence.

If intent is ambiguous, default to **`audit`**.

---

## Shared Pipeline

All variants follow this pipeline. Each variant specializes certain steps.

```
Intake → Classify Intent → Extract Contract → Design Change ──[GATE]──▶ Implement → Verify Equivalence → Cut Over
```

- **Intake** — identify the target surface: files, modules, subsystems, handlers; find tests, docs, contracts, ADRs.
- **Classify Intent** — infer user intent, artifact intent, and system intent from evidence. Select the variant via the Boundary Test above.
- **Extract Contract** — capture what must survive the change. Variant-specific: semantic contract (A), semantic surface (B), or behavioral contract (C).
- **Design Change** — choose the seam, migration shape, or strategy. Variant-specific.
- **Gate** — if the change intentionally alters semantics, boundaries, architecture, or data meaning, stop. In `audit`, report the required decision. In `execute`, **DO NOT** continue until the decision is recorded.
- **Implement** — create the new code in an isolated path first; touch old code only for seams, adapters, or routing.
- **Verify Equivalence** — prove the new path preserves the contract; map evidence to each preserved element.
- **Cut Over** — follow the cutover checklist; deprecate legacy paths when safe; document residue.

---

## Execution

Load shared references on demand:

1. **During `Verify Equivalence`:** [references/equivalence-checks.md](./references/equivalence-checks.md)
2. **During `Cut Over`:** [references/cutover-checklist.md](./references/cutover-checklist.md)
3. **When hidden control paths, value propagation, or lifecycle behavior carry semantic risk:** [references/advanced-analysis.md](./references/advanced-analysis.md)
4. **During consumer tracing:** [references/consumer-mapping.md](./references/consumer-mapping.md)

Then load variant-specific references per the sections below.

---

## Variant A: Implementation Refactor

Choose when implementation is the primary thing changing and the external surface, module boundary, and technology model stay materially stable.

### Additional references

5. **During `Classify Intent`:** [references/intent-detection.md](./references/intent-detection.md)
6. **During `Extract Contract`:** [references/semantic-contract.md](./references/semantic-contract.md) and [references/legacy-debt-handling.md](./references/legacy-debt-handling.md)
7. **During `Design Change` (seam selection):** [references/seam-selection.md](./references/seam-selection.md)
8. **During `Implement`:** [references/refactor-workflow.md](./references/refactor-workflow.md)
9. **Only if the gate opens:** [references/adr-gate.md](./references/adr-gate.md)

Use [assets/refactor-plan-template.md](./assets/refactor-plan-template.md) for `audit` output.

### Variant-specific steps

- **Extract Contract**: classify every notable legacy behavior as Intentional, Relied-upon accidental, Ambiguous, or Dead before writing the replacement.
- **Design Change**: prefer the smallest clean seam — new module behind the same interface by default, then façade, adapter, branch-by-abstraction, strangler path, or feature flag. See [references/seam-selection.md](./references/seam-selection.md).
- **Gate**: opens when the refactor changes externally visible semantics, shared boundaries, architecture, or long-lived contracts. See [references/adr-gate.md](./references/adr-gate.md).

---

## Variant B: Boundary Migration

Choose when the module, package, path, or ownership boundary is the primary thing changing and the caller-visible surface must stay materially stable.

### Additional references

5. **During `Extract Contract` (surface audit):** [references/surface-audit.md](./references/surface-audit.md)
6. **During `Design Change`:** [references/boundary-workflow.md](./references/boundary-workflow.md)
7. **Only if the gate opens:** [references/surface-break-gate.md](./references/surface-break-gate.md)

Use [assets/migration-plan-template.md](./assets/migration-plan-template.md) for `audit` output.

### Variant-specific steps

- **Extract Contract**: map the semantic surface from the caller's point of view — discovery, entry points, invocation contract, result contract, behavioral invariants, caller assumptions. Classify each element as Required, Relied-on incidental, Ambiguous, or Dead. See [references/surface-audit.md](./references/surface-audit.md).
- **Design Change**: prefer the smallest safe shape — stable re-export, facade, adapter, branch-by-abstraction, or staged caller migration. See [references/boundary-workflow.md](./references/boundary-workflow.md).
- **Implement**: migrate callers in cohorts (internal direct → indirect or generated → external or boundary).
- **Gate**: opens when the migration changes externally visible surface semantics, published paths, or ownership boundaries. See [references/surface-break-gate.md](./references/surface-break-gate.md).

---

## Variant C: Technology Migration

Choose when the technology model itself is changing — framework, runtime, major version, module system, or programming model — and behavioral semantics must survive an intentionally changing technical surface.

### Additional references

5. **During `Extract Contract`:** [references/behavioral-contract.md](./references/behavioral-contract.md)
6. **During `Design Change` (breaking changes):** [references/breaking-change-inventory.md](./references/breaking-change-inventory.md)
7. **During `Design Change` (strategy):** [references/tech-strategy.md](./references/tech-strategy.md)
8. **During `Implement` (compatibility layers):** [references/compatibility-layer.md](./references/compatibility-layer.md)

Use [assets/migration-plan-template.md](./assets/migration-plan-template.md) for `audit` output.

### Variant-specific steps

- **Extract Contract**: extract the behavioral contract — what the code **does**, not how it is called. Surface change is expected; the contract guards behavioral semantics only. See [references/behavioral-contract.md](./references/behavioral-contract.md).
- **Design Change**: classify every breaking change as Surface-Only, Behavioral, or Ambiguous before choosing a strategy. Check official migration guides and codemods first. See [references/breaking-change-inventory.md](./references/breaking-change-inventory.md) and [references/tech-strategy.md](./references/tech-strategy.md).
- **Implement**: build compatibility layers for coexistence. Every bridge must have a named owner, removal condition, and expected lifetime. See [references/compatibility-layer.md](./references/compatibility-layer.md).
- **Gate**: Behavioral and Ambiguous breaking changes stop until a decision is recorded for each. Surface-Only changes proceed.

---

## Core Rules

- **Semantics first**: preserve externally observable meaning, invariants, and domain intent unless an explicit decision says otherwise.
- **Contract before code**: extract the semantic contract, semantic surface, or behavioral contract before touching any file.
- **Intent is inferred, not guessed**: rank evidence as user constraints → tests → contracts → call sites → docs → comments → code shape.
- **State the preservation boundary**: declare which layers must stay stable before writing replacement code.
- **Classify before porting**: mark every notable legacy behavior or breaking change before implementation begins.
- **Replace, do not accrete**: prefer a new module or isolated path over more branches in legacy code.
- **Bridge minimally**: touch legacy code only for seams, adapters, compatibility layers, or cutover points.
- **Migrate in cohorts**: group callers by risk; move them in order (internal → indirect → external). Never do a single unbounded sweep.
- **Prove equivalence**: map evidence to preserved elements; inspection alone is not enough.
- **Gate intentional change**: if semantics, boundaries, architecture, or behavioral contract change, stop and get the decision recorded.
- **Surface change is expected in technology migration**: the gate guards behavioral semantics, not API shape.
- **Compatibility layers are temporary**: every shim, polyfill, or adapter must have a named owner, removal condition, and documented expected lifetime.
- **Expose residue**: call out remaining legacy paths, deprecated modules, partial migrations, and debt carry-forwards.

---

## Deliverables

### `audit`

Return using the appropriate plan template ([refactor-plan-template.md](./assets/refactor-plan-template.md) or [migration-plan-template.md](./assets/migration-plan-template.md)):

- detected intent with confidence and evidence sources
- stated preservation boundary
- legacy behavior inventory with classes, or breaking change inventory with classifications
- semantic contract, semantic surface, or behavioral contract
- replacement seam, migration shape, or strategy recommendation
- equivalence plan
- risk list and debt carry-forwards
- gate requirement if the change is not purely semantic-preserving

Do **not** edit files.

### `execute`

Deliver:

- new or rewritten module(s) implementing the preserved contract
- minimal bridging edits in legacy call sites or compatibility layers
- equivalence evidence mapped to preserved elements
- completed cutover checklist with rollback notes
- gate decision record if semantics or architecture intentionally changed
- migration notes for deprecated paths, names, or bridges

---

## Verification

- [ ] Existing behavior, interfaces, and constraints were read before redesigning.
- [ ] Intent was inferred from evidence in priority order.
- [ ] The preservation boundary was stated before implementation.
- [ ] Legacy behaviors or breaking changes were classified before porting.
- [ ] A contract was written down before implementation.
- [ ] The replacement was implemented in a new or isolated path where feasible.
- [ ] Legacy code was edited only for seam creation, bridging, or cutover.
- [ ] Caller migration happened by cohorts, not as an unbounded sweep.
- [ ] Equivalence evidence is mapped to the preserved elements.
- [ ] Cutover followed the checklist; rollback plan was defined.
- [ ] Intentional semantic or architectural changes triggered the gate.
- [ ] Compatibility layers have a named owner, removal condition, and expected lifetime.
- [ ] Remaining legacy paths, deprecated names, and debt carry-forwards are called out.

---

## Final Standard

Result: the agent makes the change while proving the system still means and does the same thing, leaves a clear record of every decision and residual debt, and never smuggles a behavioral change past the gate.
