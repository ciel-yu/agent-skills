# Breaking Change Inventory

Classify every breaking change before migration begins. Do not discover breaking changes mid-implementation.

---

## Sources

Check in this order:

1. **Official migration guide** — the target tech's documented upgrade path
2. **Changelog / release notes** — for version-to-version migrations
3. **Codemod tools** — automated transforms that enumerate known breaking changes
4. **Deprecation warnings** — runtime or compiler warnings firing against the current codebase
5. **Type errors or lint errors** — surface-level breakage when the new version is installed locally
6. **Community migration resources** — widely-used upgrade guides, blog posts, or ecosystem tooling

---

## Classification

For each breaking change, assign one class:

| Class | Meaning | Gate required? |
|---|---|---|
| **Surface-Only** | API shape, method name, import path, or syntax changes; behavior is identical | No — proceed |
| **Behavioral** | Logic, data semantics, error handling, ordering, or lifecycle actually differs | Yes — stop |
| **Ambiguous** | Cannot determine from docs alone whether behavior changes | Yes — resolve first |

Surface-Only changes proceed to consumer tracing and cohort migration.
Behavioral and Ambiguous changes stop at the Behavioral-Risk Gate until a decision is recorded for each.

---

## Automation check

For each breaking change, ask:

- Is there an **official codemod**? (e.g., `@next/codemod`, `react-codemod`, `jscodeshift` transforms)
- Is there a **community codemod** or migration script?
- Can it be handled by a **lint rule or type-check** alone?
- Must it be **manual**?

Prefer automated migration where reliable. Document why a change is manual when automation was available but skipped.

---

## Inventory template

| Change | Source | Class | Affected consumers | Automation | Decision / action |
|---|---|---|---|---|---|
| `componentWillMount` removed | React 18 guide | Surface-Only | 12 components | `react-codemod` | Replace with `useEffect(() => {}, [])` |
| Concurrent mode rendering order | React 18 release notes | Behavioral | 3 components | None | Gate: decide if new order is acceptable |
| `require()` not supported | ESM spec | Surface-Only | 47 files | `cjs-to-esm` | Replace with `import` |

---

## Residual state

After classification:

- All **Behavioral** and **Ambiguous** items must be resolved at the gate before migration continues.
- All **Surface-Only** items may proceed but require consumer tracing and cohort migration.
- **Unclassified** items must be classified; "unknown" is not an acceptable status going into implementation.
