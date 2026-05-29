---
name: semantic-surface-module-migration
description: "Use when the user wants to move, split, consolidate, or replace a code module by preserving its semantic surface: import paths, callable/API shape, data contracts, side effects, errors, and caller expectations. Triggers: migrate a module to a new package, extract a subsystem behind a stable facade, move callers off a legacy path, or split a god module without breaking consumers. Do not use for tiny local edits, purely mechanical renames with no migration risk, or intentional public API redesign without an explicit surface-change decision."
---

# Semantic Surface Module Migration

This skill is a **Pipeline + Reviewer** for module migrations driven by semantic surface, not file layout. Migrate by first defining what callers observe and rely on, then move implementation and callers in stages while preserving that surface or explicitly gating any intentional break.

---

## Use When

Use when the user asks to:

- move a module to a new package, directory, layer, or ownership boundary without breaking callers
- split a large module into smaller modules while keeping the same public surface
- consolidate duplicate modules under one canonical surface and migrate callers safely
- migrate consumers from a legacy path, adapter, or facade to a new module boundary in stages
- preserve how a module is discovered and used even when its storage location or owning boundary changes

Do **not** use for:

- tiny local fixes where migration discipline is unnecessary
- purely mechanical renames or moves with no meaningful semantic-risk surface
- brand-new module design with no existing callers or compatibility constraints
- intentional public API or domain-contract redesign without an explicit decision to change the surface
- implementation rewrites where the goal is cleaner logic behind a stable interface with no structural change (use `semantic-safe-refactor`)
- migrations where the technical surface intentionally changes due to a framework upgrade or convention change (use `tech-migration`)

---

## Operating Modes

- **`audit`** - map the current semantic surface, callers, migration risks, and recommended migration shape without editing files
- **`migrate`** - implement the target module boundary, add the minimum compatibility layer, migrate callers, and prove the preserved surface still holds

If intent is ambiguous, default to **`audit`**.

---

## Pipeline

`Intake → Map Semantic Surface → Trace Consumers & Discovery Paths → Choose Migration Shape ──[Surface-Break Gate]──▶ Implement Target Boundary → Migrate Callers by Cohort → Verify Surface Equivalence → Cut Over`

- **Intake** - identify the module, its current location, target location or boundary, caller scope, and any rollout constraints.
- **Map Semantic Surface** - write down what consumers actually observe: import path, exported names, callable signatures, types or schema shape, sync/async behavior, errors, side effects, ordering, initialization, and any relied-on performance or lifecycle properties.
- **Trace Consumers & Discovery Paths** - find direct imports, dynamic loading, DI wiring, registries, config references, generated code, tests, docs, and scripts that discover or rely on the surface.
- **Choose Migration Shape** - prefer the smallest safe shape: stable re-export, facade, adapter, branch-by-abstraction, dual-write/read boundary, or staged caller migration.
- **Surface-Break Gate** - if the migration changes externally visible surface semantics, shared ownership boundaries, published paths, or data meaning, stop. In `audit`, report the required decision. In `migrate`, **DO NOT** continue until that decision exists.
- **Implement Target Boundary** - create the target module or façade first; edit the legacy module only to add bridging exports, routing, adapters, or deprecation notices.
- **Migrate Callers by Cohort** - move consumers in deterministic groups: internal direct callers first, then indirect or generated consumers, then external entry points.
- **Verify Surface Equivalence** - prove that the preserved surface still behaves the same for each caller cohort.
- **Cut Over** - remove temporary bridges only when callers are fully migrated and residue is documented.

Do **not** start by moving files and then hoping the surface still matches.

---

## Execution

Load on demand:

1. **During `Map Semantic Surface`:** [references/surface-audit.md](./references/surface-audit.md)
2. **During `Trace Consumers & Discovery Paths`:** [references/consumer-mapping.md](./references/consumer-mapping.md)
3. **During `Choose Migration Shape` and `Implement Target Boundary`:** [references/migration-workflow.md](./references/migration-workflow.md)
4. **During `Verify Surface Equivalence`:** [references/equivalence-checks.md](./references/equivalence-checks.md)
5. **During `Cut Over`:** [references/cutover-checklist.md](./references/cutover-checklist.md)

Use [assets/migration-plan-template.md](./assets/migration-plan-template.md) as the output format for `audit` mode.

Read both the current module and representative consumers before choosing the migration shape.

---

## Core Rules

- **Surface first**: define the semantic surface before editing module boundaries.
- **Observed behavior outranks file structure**: preserve what callers import, call, receive, and rely on unless an explicit decision says otherwise.
- **Consumers are evidence**: infer the real surface from call sites, tests, runtime wiring, docs, and generated artifacts, not from exports alone.
- **Migrate in cohorts**: group callers by risk and discovery path; do not mix unrelated migrations into one blind sweep.
- **Bridge minimally**: temporary shims, re-exports, adapters, and facades exist only to preserve the surface during migration.
- **Keep one canonical target**: after migration, there should be one owning module boundary, even if temporary compatibility paths remain.
- **Prove equivalence on the surface**: verification must target caller-observable behavior, not internal similarity.
- **Gate intentional breakage**: if import paths, API names, error shape, async behavior, or other surface semantics intentionally change, stop and get the decision recorded.
- **Expose residue**: name leftover shims, deprecated entry points, partial caller cohorts, and cleanup still required.

---

## Deliverables

### `audit`

Return using [assets/migration-plan-template.md](./assets/migration-plan-template.md):

- current semantic surface
- consumer/discovery map
- migration shape recommendation with rationale
- caller cohort plan
- surface-equivalence plan
- risk list and temporary bridge plan
- explicit gate note if the migration is not surface-preserving

Do **not** edit files.

### `migrate`

Deliver:

- target module(s) or facade implementing the preserved surface
- minimal compatibility edits in legacy modules or import paths
- migrated caller cohorts with notes on what remains
- equivalence evidence mapped to the semantic surface
- cutover notes and cleanup residue
- explicit decision reference if any intentional surface break occurred

---

## Verification

- [ ] The module and representative consumers were read before migration design.
- [ ] The semantic surface was written down before files moved.
- [ ] Consumer and discovery paths were mapped beyond direct imports.
- [ ] The migration shape matches the smallest safe surface-preserving option.
- [ ] Legacy modules were edited only for bridging, routing, or deprecation.
- [ ] Caller migration happened by cohorts, not as an unbounded sweep.
- [ ] Surface-equivalence evidence covers imports, behavior, data shape, errors, and side effects that callers rely on.
- [ ] Any intentional surface break triggered the gate instead of being smuggled into the migration.
- [ ] Remaining shims, deprecated paths, and follow-up cleanup are called out.

---

## Final Standard

Result: the agent migrates modules by preserving or explicitly gating the semantic surface callers depend on, so code ownership and structure can change without silent breakage.
