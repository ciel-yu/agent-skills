# Surface-Break Gate

Use this gate only when the migration is no longer surface-preserving.

---

## Open the gate when the migration intentionally changes

- import paths or module-discovery routes that external consumers depend on
- exported names, function signatures, or type shapes at a shared boundary
- error types, error codes, or retry/failure semantics callers rely on
- async or lifecycle behavior at any discovery point callers use
- ownership boundaries in a way that affects how other teams or systems reach this module

If the change is purely internal and callers observe the same surface, do not open the gate.

---

## Migration-safe vs gate-required

### Usually migration-safe

- moving internal implementation files with no shared consumers
- renaming private, unexported symbols
- reorganizing internal module structure behind a stable re-export
- adding new entry points alongside stable existing ones
- adding fields to output objects where callers tolerate extras

### Usually gate-required

- removing or renaming exported entry points callers currently use
- changing argument shape, required parameters, or return type in ways callers will notice
- splitting or redirecting a canonical import path that callers depend on for discovery
- removing errors, changing error types, or altering retry or failure semantics
- changing async behavior or initialization order in an observable way

---

## Minimal decision record for a surface break

```md
# Surface Change Decision: <module or boundary>

- Date:
- Affected import paths / entry points:
- What is changing:
- Why this change is intentional:
- Callers / cohorts affected:
- Compatibility bridge plan (if any):
- Cleanup target:
```

Keep the record focused on the surface delta, not the mechanical file move.
