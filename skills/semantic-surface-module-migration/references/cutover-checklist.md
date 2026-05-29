# Cutover Checklist

- Confirm every caller cohort is either migrated or intentionally covered by a compatibility bridge.
- Confirm the canonical target module boundary is documented.
- Mark legacy entry points as deprecated if they still exist.
- Remove only the shims made unnecessary by the completed migration.
- Record remaining residue: old paths, adapters, TODO cleanup, external consumers still pending.
- State rollback direction: restore prior routing, restore previous exports, or revert caller cohort migration.
- Re-run discovery to ensure no hidden import path still points at removed code.
