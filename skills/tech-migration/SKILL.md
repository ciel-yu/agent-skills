---
name: tech-migration
description: "Use when migrating code between different technology frameworks, different versions of the same framework, or different code conventions while preserving behavioral and domain semantics. Triggers: upgrade a major framework version (React 16 → 18, Angular 12 → 15), migrate between libraries or runtimes (Express → Fastify, Webpack → Vite), convert code conventions across a codebase (CommonJS → ESM, callbacks → async/await, class components → hooks). The technical surface intentionally changes; the goal is to preserve what the code means and does. Do not use when the public surface must stay identical (use semantic-surface-module-migration) or when only internals change behind a stable interface (use semantic-safe-refactor)."
---

# Tech Migration

This skill is a **Pipeline + Reviewer** for technology migrations where the **surface intentionally changes** but behavioral and domain semantics must be preserved. Unlike `semantic-safe-refactor` (surface stable, internals change) and `semantic-surface-module-migration` (surface preserved, boundary moves), tech migration treats surface transformation as expected and guards behavioral equivalence as the single invariant.

---

## Use When

Use when the user asks to:

- upgrade a major framework or library version with breaking API changes
- migrate from one framework or runtime to another
- convert code conventions across a codebase (module system, async pattern, component model)
- replace a third-party dependency with an alternative while preserving behavior
- adopt a new language feature or compilation target (ES5 → ES2022, Python 2 → 3)

Do **not** use for:

- migrations where callers must not see any surface change (use `semantic-surface-module-migration`)
- internal rewrites behind a fully stable interface (use `semantic-safe-refactor`)
- greenfield rewrites with no behavioral baseline to preserve
- purely mechanical renames or moves with no behavioral risk
- cases where both module structure and implementation need to change simultaneously without a tech surface change — compose `semantic-surface-module-migration` (for the boundary) with `semantic-safe-refactor` (for the implementation) instead

---

## Operating Modes

- **`audit`** — map the behavioral contract, inventory breaking changes, trace consumers, recommend a migration strategy; do not edit files
- **`migrate`** — implement the migration, add compatibility layers where needed, migrate cohorts, prove behavioral equivalence

Default to **`audit`** when intent is ambiguous.

---

## Pipeline

`Intake → Map Behavioral Contract → Inventory Breaking Changes → Trace Consumers ──[Behavioral-Risk Gate]──▶ Choose Migration Strategy → Implement Compatibility Layer → Migrate by Cohort → Verify Behavioral Equivalence → Cut Over`

- **Intake** — identify: source tech + version, target tech + version, scope (file, module, package, repo), rollout constraints, and any coexistence requirements.
- **Map Behavioral Contract** — extract what must survive the migration: business logic, domain rules, data transformations, error semantics, side effects, user-visible behavior, and any performance or lifecycle guarantees callers depend on. This is the only preservation invariant; the surface is allowed to change.
- **Inventory Breaking Changes** — collect the official migration guide plus discovered gaps; classify every breaking change as Surface-Only (API shape changes, behavior is identical), Behavioral (logic or semantics actually differ), or Ambiguous.
- **Trace Consumers & Discovery Paths** — find all usages of APIs, imports, patterns, or conventions being migrated; group into cohorts by risk and replaceability.
- **Behavioral-Risk Gate** — Surface-Only changes proceed. Behavioral or Ambiguous changes stop: in `audit`, report them as decisions required; in `migrate`, **DO NOT** continue until each decision is recorded.
- **Choose Migration Strategy** — select: codemod-first, manual, incremental (strangler / branch-by-abstraction / feature flag), big-bang (only when scope and equivalence coverage justify it), or dual-version coexistence.
- **Implement Compatibility Layer** — build the minimum shims, polyfills, adapters, or version bridges needed to let old callers and new implementation coexist; every bridge must have a named owner, removal condition, and expected lifetime.
- **Migrate by Cohort** — move consumer groups in order: isolated internal callers first, then indirect or generated consumers, then external or boundary consumers.
- **Verify Behavioral Equivalence** — prove that each cohort's behavior is unchanged despite the surface transformation; map evidence to the behavioral contract.
- **Cut Over** — retire compatibility layers only after all cohorts are migrated and the observation window has passed; document residue.

Do **not** move files or rewrite call sites before the behavioral contract is written and the breaking changes are classified.

---

## Execution

Load on demand:

1. **During `Map Behavioral Contract`:** [references/behavioral-contract.md](./references/behavioral-contract.md)
2. **During `Inventory Breaking Changes`:** [references/breaking-change-inventory.md](./references/breaking-change-inventory.md)
3. **During `Trace Consumers`:** [references/consumer-mapping.md](./references/consumer-mapping.md)
4. **During `Choose Migration Strategy`:** [references/migration-strategy.md](./references/migration-strategy.md)
5. **During `Implement Compatibility Layer`:** [references/compatibility-layer.md](./references/compatibility-layer.md)
6. **During `Verify Behavioral Equivalence`:** [references/equivalence-checks.md](./references/equivalence-checks.md)
7. **During `Cut Over`:** [references/cutover-checklist.md](./references/cutover-checklist.md)

Use [assets/migration-plan-template.md](./assets/migration-plan-template.md) as the output format for `audit` mode.

Read existing code and representative consumers before choosing a strategy. Check for an official migration guide and available codemods before designing custom tooling.

---

## Core Rules

- **Surface change is expected**: the gate guards behavioral semantics, not API shape.
- **Contract before code**: extract the behavioral contract before touching any file.
- **Breaking changes are first-class**: classify every breaking change before migration begins; do not discover them mid-implementation.
- **Official guide first**: consult the target tech's official migration documentation and known codemods before designing a custom approach.
- **Compatibility layers are temporary**: every shim, polyfill, or adapter must have a named owner, a removal condition, and a documented expected lifetime.
- **Migrate in cohorts**: never do a single unbounded sweep; group consumers by risk and move them in order.
- **Prove behavioral equivalence**: surface change does not imply behavioral equivalence; evidence must map to the behavioral contract, not to surface shape.
- **Gate behavioral risk**: any breaking change that alters logic, data semantics, error handling, or lifecycle needs an explicit decision before migration proceeds.
- **Expose residue**: name remaining compatibility layers, partially migrated cohorts, and cleanup debt after cutover.

---

## Deliverables

### `audit`

Return using [assets/migration-plan-template.md](./assets/migration-plan-template.md):

- behavioral contract (what must be preserved)
- breaking change inventory with classifications
- consumer and discovery path map with cohorts
- recommended migration strategy with rationale
- compatibility layer design and bridge registry
- cohort migration plan
- behavioral equivalence plan
- behavioral-risk gate items (decisions required before migration)
- risk list and residual debt

Do **not** edit files.

### `migrate`

Deliver:

- migrated implementation(s) per cohort
- compatibility layer code with documented owner, removal condition, and lifetime
- behavioral equivalence evidence mapped to the contract
- record of each behavioral-risk gate decision
- cutover notes and cleanup residue
- removal plan for compatibility bridges

---

## Verification

- [ ] Source tech, target tech, scope, and rollout constraints were identified before starting.
- [ ] Behavioral contract was written before any files were changed.
- [ ] Official migration guide and available codemods were checked before custom tooling.
- [ ] Every breaking change was classified (Surface-Only / Behavioral / Ambiguous) before migration.
- [ ] Behavioral and Ambiguous breaking changes triggered the gate; no behavioral change was smuggled in silently.
- [ ] Consumers were traced beyond direct imports: config, generated code, DI, scripts.
- [ ] Migration happened by cohort, not as an unbounded sweep.
- [ ] Each compatibility layer has a documented owner, removal condition, and expected lifetime.
- [ ] Behavioral equivalence evidence maps to the behavioral contract, not to surface similarity.
- [ ] Remaining bridges, partial cohorts, and debt carry-forwards are named.

---

## Final Standard

Result: the agent migrates the technical surface while proving the code still means and does the same thing, leaves every compatibility bridge explicitly temporary with a named removal condition, and records every decision where behavioral semantics were at risk.
