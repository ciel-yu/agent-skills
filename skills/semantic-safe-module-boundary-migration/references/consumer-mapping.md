# Consumer Mapping

Trace every meaningful consumer class before migration.

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

Group consumers into cohorts:

1. **Internal direct callers** - easiest to migrate first.
2. **Indirect or generated callers** - require more tooling awareness.
3. **External or boundary callers** - published surfaces, integrations, public packages, or shared platform contracts.

If a cohort cannot be migrated immediately, decide whether it needs a temporary facade, re-export, or adapter.
