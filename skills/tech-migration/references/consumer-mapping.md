# Consumer Mapping

Trace every usage of the APIs, patterns, or conventions being migrated before choosing a strategy.

---

## What to find

- direct imports of modules or packages being replaced
- usage of specific API methods, hooks, components, or patterns being removed or renamed
- syntax patterns being converted (e.g., `require()`, `Promise.then()`, class lifecycle methods)
- configuration files referencing the old tech (webpack config, babel config, tsconfig, framework config)
- generated code, codegen templates, or schema bindings that emit old-tech patterns
- scripts, CLIs, and tooling that depend on old-tech behaviors
- tests that lock in old-tech-specific behavior or API shape
- documentation examples that function as usage contracts

---

## Cohort classification

Group consumers by migration risk and dependency order:

| Cohort | Description | Migrate |
|---|---|---|
| **A — Isolated internal** | Self-contained, no shared callers, easy to verify in isolation | First |
| **B — Shared internal** | Used by multiple internal modules; medium blast radius | Second |
| **C — Generated / indirect** | Machine-generated, dynamically loaded, or config-driven | Third |
| **D — Boundary / external** | External consumers, published packages, cross-team or cross-service dependencies | Last |

Do not migrate an outer cohort until the inner cohort is stable and verified.

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
- Cohort A (isolated internal): N files — [list or search command]
- Cohort B (shared internal): N files — [list or search command]
- Cohort C (generated / indirect): N files — [list or search command]
- Cohort D (boundary / external): N files — [list or search command]
- Automation available: Yes / Partial / No
- Tooling: [codemod name, lint rule, etc.]
```
