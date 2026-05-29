# Cutover Checklist

Use during `Cut Over`.

---

## Pre-cutover: before removing any old path

- [ ] Surface-equivalence evidence is complete for all Required surface elements.
- [ ] Every caller cohort is either fully migrated or covered by a documented compatibility bridge.
- [ ] The canonical target module boundary is defined and documented.
- [ ] Rollback plan is defined: how to restore the old discovery path or re-export if needed.
- [ ] Legacy entry points are still reachable for immediate rollback.
- [ ] Monitoring or test coverage exists for the new boundary.
- [ ] If cutover affects shared or external consumers, those teams are notified.
- [ ] Partial migration scope is documented if not all cohorts switch at once.

**DO NOT** remove old paths if equivalence evidence is incomplete or rollback is undefined.

---

## Cutover: switching callers

- [ ] Switch each cohort to the new boundary at the defined entry point.
- [ ] Confirm the new boundary resolves correctly for all switched callers.
- [ ] Verify behavior through the new discovery path under representative conditions.
- [ ] Keep old entry points and compatibility bridges available for immediate rollback.

---

## Post-cutover: stabilize and clean up

- [ ] Old entry points and shims have been unused for a safe observation period.
- [ ] Legacy import paths are deprecated or removed.
- [ ] Compatibility shims, re-exports, and adapters are removed when no longer needed.
- [ ] Docs, READMEs, and inline comments updated to reflect the new canonical path.
- [ ] Debt carry-forwards are recorded: remaining shims, pending cohort migrations, follow-up cleanup.
- [ ] Any intentional surface break is referenced in the migration record.

---

## Rollback: when and how

Trigger rollback when:

- the new boundary fails to resolve for a consumer cohort
- callers receive wrong outputs, missing symbols, or unexpected errors through the new path
- a side effect is missing or fires incorrectly on the new path
- a compatibility shim is removed before its callers are fully migrated

To roll back:

- restore the old module location or re-enable the compatibility re-export
- confirm callers import and behave correctly through the restored path
- document what failed and why
- do not reattempt cutover until the failure is explained and equivalence is re-verified
