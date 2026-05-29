# Compatibility Layer

A compatibility layer is temporary infrastructure that lets old callers and new tech coexist during migration. Every bridge must be explicitly temporary with a named removal condition.

---

## Types

| Type | When to use | Example |
|---|---|---|
| **Re-export shim** | Old import path must remain valid temporarily | `export { newFn as oldFn } from './new-module'` |
| **Polyfill** | Runtime feature absent in the current environment | Fetch polyfill, Promise polyfill |
| **Adapter** | Old calling convention must translate to new API shape | Wrapping a callback-based API in a Promise |
| **Version bridge** | Two versions of the same library must coexist | `react-dom/client` vs `react-dom` during partial upgrade |
| **Facade** | Unified interface routing to old or new implementation | Service interface delegating based on feature flag |
| **Deprecation wrapper** | Old API remains callable but signals intent to remove | Function that delegates to new fn and emits a deprecation warning |

---

## Mandatory properties for each bridge

Every compatibility layer must document:

- **Owner** — who is responsible for removing it
- **Removal condition** — the exact condition that must be true before removal (e.g., "all Cohort B callers migrated", "feature flag retired")
- **Expected lifetime** — rough milestone or date
- **Callers still relying on it** — list or search command to find them

If any of these are missing, the bridge is indefinite debt, not a migration tool.

---

## Avoid

- Compatibility layers that silently change behavior (they are surface bridges, not behavioral transforms)
- Bridges with no named owner or removal condition
- Nested or stacked bridges (bridge on top of bridge on top of bridge)
- Bridges that bypass the behavioral contract by passing the surface check while changing outcomes

---

## Bridge registry template

Track all active bridges in the migration plan:

| Bridge | Type | Owner | Removal condition | Expected lifetime | Remaining callers |
|---|---|---|---|---|---|
| `src/compat/old-http.ts` | Adapter | @team-infra | All internal callers on new client | Q3 milestone | 7 |
| `src/compat/legacy-events.ts` | Re-export | @team-platform | `legacy-events` package removed | After v2 release | 3 |
