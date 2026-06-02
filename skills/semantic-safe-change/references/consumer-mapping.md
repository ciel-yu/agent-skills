# Consumer Mapping

Trace every meaningful consumer before migration — whether the migration is a boundary move or a technology switch.

---

## What to find

Check for:

- direct imports and re-exports
- barrel files and package export maps
- dynamic imports and lazy loading
- dependency injection containers, registries, or plugin systems
- config-driven lookups and string-based references
- generated code, codegen templates, and schema bindings
- tests that lock behavior or discovery paths
- scripts, CLIs, jobs, and tooling integrations
- documentation examples that function as user-facing contracts
- (Technology migration) usage of specific API methods, hooks, components, or patterns being removed or renamed
- (Technology migration) syntax patterns being converted (e.g., `require()`, `Promise.then()`, class lifecycle methods)
- (Technology migration) configuration files referencing the old tech (webpack, babel, tsconfig, framework config)

---

## Cohort classification

Group consumers by migration risk and dependency order:

| Cohort | Description | Migrate |
|---|---|---|
| **A — Internal direct callers** / **Isolated internal** | Self-contained, direct imports, easiest to verify | First |
| **B — Indirect or generated** / **Shared internal** | Dynamic loading, generated code, shared across modules | Second |
| **C — External or boundary** | Published surfaces, integrations, public packages, cross-team or cross-service dependencies | Last |

Do not migrate an outer cohort until the inner cohort is stable and verified.

If a cohort cannot be migrated immediately, decide whether it needs a temporary facade, re-export, or adapter.

---

## Automation options

- **Static analysis**: `grep`, `ast-grep`, `eslint` with custom rules, `codemod --dry-run`
- **Type checking**: TypeScript errors after installing the new version
- **Dependency graph**: package dependency analysis to find indirect consumers
- **Codemod dry runs**: official codemods with `--dry-run` to surface affected file counts

---

## Output format

```md
## Consumer Map
- Total consumers: N
- Cohort A: N files — [list or search command]
- Cohort B: N files — [list or search command]
- Cohort C: N files — [list or search command]
- Automation available: Yes / Partial / No
- Tooling: [codemod name, lint rule, etc.]
```
