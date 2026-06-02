# Cutover Checklist

Use during `Cut Over` for all variants.

---

## Pre-cutover: before switching traffic or callers

- [ ] Equivalence evidence is complete for all preserved layers or surface elements.
- [ ] Every caller cohort is either fully migrated or covered by a documented compatibility bridge.
- [ ] Blast radius is mapped: which callers, consumers, or systems will be affected.
- [ ] Rollback plan is defined and tested where possible.
- [ ] Legacy path is still running and reachable for immediate rollback.
- [ ] New path has been exercised under representative load or test conditions.
- [ ] Monitoring and alerting cover the new path.
- [ ] Stakeholders are notified if the cutover is user-visible or affects shared integrations.
- [ ] Partial migration scope is documented if not all callers switch at once.
- [ ] (Technology migration) All Behavioral-Risk Gate decisions are recorded; no Ambiguous breaking changes remain unresolved.
- [ ] (Technology migration) Every compatibility bridge has a named owner and a removal condition.

**DO NOT** switch traffic if equivalence evidence is incomplete or rollback is undefined.

---

## Cutover: switching traffic or callers

- [ ] Switch each cohort to the new path or boundary at the defined entry point.
- [ ] Confirm the new path handles live traffic without errors or regressions.
- [ ] Verify side effects and behavior on the new path match the preserved contract.
- [ ] Keep the legacy path available for immediate rollback.
- [ ] (Technology migration) Remove or deprecate old-tech entry points; update CI/CD where applicable.

---

## Post-cutover: stabilize and clean up

- [ ] Legacy path has been stable and unused for a safe observation period.
- [ ] Legacy entry points are deprecated or removed.
- [ ] Compatibility shims, re-exports, and adapters are removed when no longer needed.
- [ ] ADR is updated if the change affected semantics or architecture.
- [ ] Docs, READMEs, and inline comments are updated to reflect the new path.
- [ ] Debt carry-forwards are recorded and tracked: remaining shims, pending cohort migrations, follow-up cleanup.
- [ ] Remaining deprecated names, modules, or compatibility bridges are called out explicitly.
- [ ] (Technology migration) Old tech dependency removed from package manifests if fully replaced.

---

## Rollback: when and how

Roll back immediately when:

- the new path produces unexpected errors, wrong outputs, or missing side effects
- equivalence breaks in production before the full observation period
- a downstream consumer fails in a way not covered by pre-cutover evidence
- a compatibility shim is removed before its callers are fully migrated
- (Technology migration) new tech produces behavioral differences not accounted for in equivalence evidence

Rollback steps:

1. revert callers to the legacy entry point or re-enable the relevant compatibility layer
2. confirm the legacy path is producing expected results
3. document what failed and why
4. do not re-attempt cutover until the failure is explained and equivalence is re-verified
