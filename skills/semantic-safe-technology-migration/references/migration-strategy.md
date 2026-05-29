# Migration Strategy

Choose a strategy based on scope, behavioral risk, consumer count, and coexistence requirements. Check for official codemods and migration tooling before designing a custom approach.

---

## Strategy options

### 1. Codemod-first

Use when official or community codemods cover the majority of breaking changes.

- Run codemods on the full scope first.
- Manually handle residual changes codemods cannot cover.
- Review codemod output before committing; do not assume automation is always correct.
- Best for: large codebases with well-characterized Surface-Only breaking changes and official tooling (e.g., major framework version upgrades with published codemods).

### 2. Incremental — Strangler

Use when old and new tech can coexist at a boundary (route, service, component, package).

- Introduce the new tech at one boundary.
- Gradually move consumers from the old path to the new path.
- Keep the old path alive until the last consumer migrates.
- Best for: framework-to-framework migrations, replacing a library in a large app.

### 3. Incremental — Branch-by-abstraction

Use when a shared abstraction can decouple consumers from the specific tech in use.

- Introduce an interface or abstraction layer that both old and new tech can implement.
- Implement the new tech behind it.
- Migrate consumers to the abstraction, then switch the underlying implementation.
- Best for: replacing a data access layer, swapping an HTTP client, changing a state management library.

### 4. Incremental — Feature flag / dual-version

Use when new and old tech must coexist per-consumer or per-feature during rollout.

- Use a flag or configuration to route between old and new.
- Migrate features or consumers behind the flag.
- Remove the flag and old path after full rollout.
- Best for: runtime upgrades requiring gradual rollout, A/B coexistence.

### 5. Big-bang

Use when scope is small, equivalence coverage is high, and rollback is fast.

- Migrate everything in one pass.
- Requires complete equivalence evidence before cutover.
- Requires a fast, tested rollback path.
- Avoid for large scopes or when behavioral equivalence is difficult to prove upfront.

---

## Selection criteria

| Factor | Prefer incremental | Prefer big-bang |
|---|---|---|
| Scope | Large (many files, teams, consumers) | Small, isolated |
| Behavioral risk | High (Behavioral breaking changes present) | Low (mostly Surface-Only) |
| Coexistence support | Target tech can run alongside old | Target tech cannot coexist |
| Rollback cost | High (external deps, data migration) | Low (local, reversible) |
| Equivalence coverage | Partial | Complete |
| Team size | Multiple teams or codebases | Single team |

---

## Cohort migration order

Regardless of strategy, always migrate in this order:

1. **Cohort A** — isolated internal consumers; easiest to verify
2. **Cohort B** — shared internal consumers; medium risk
3. **Cohort C** — generated or indirect consumers; require tooling awareness
4. **Cohort D** — boundary or external consumers; highest risk

Do not proceed to an outer cohort until the inner cohort is stable.

---

## Automation inventory

Before starting, record:

| Breaking change | Codemod available | Tool | Reliability | Notes |
|---|---|---|---|---|
| | Yes / No / Partial | | High / Medium / Low | |
