# Cutover Checklist

Use this reference during `Cut Over`.

---

## Pre-cutover: before switching traffic

- [ ] Equivalence evidence is complete for all preserved layers.
- [ ] Blast radius is mapped: which callers, consumers, or systems will be affected.
- [ ] Rollback plan is defined and tested where possible.
- [ ] Legacy path is still running and reachable.
- [ ] New path has been exercised under representative load or test conditions.
- [ ] Monitoring and alerting cover the new path.
- [ ] Stakeholders are notified if the cutover is user-visible or affects shared integrations.
- [ ] Partial migration scope is documented if not all callers switch at once.

**DO NOT** switch traffic if equivalence evidence is incomplete or rollback is undefined.

---

## Cutover: switching traffic

- [ ] Switch callers to the new path at the defined entry point.
- [ ] Confirm the new path handles live traffic without errors or regressions.
- [ ] Verify side effects on the new path match the preserved contract.
- [ ] Keep the legacy path available for immediate rollback.

---

## Post-cutover: stabilize and clean up

- [ ] Legacy path has been stable and unused for a safe observation period.
- [ ] Legacy entry points are deprecated or removed.
- [ ] Compatibility shims are removed when no longer needed.
- [ ] ADR is updated if the refactor changed semantics or architecture.
- [ ] Docs, READMEs, and inline comments are updated to reflect the new path.
- [ ] Debt carry-forwards are recorded and tracked.
- [ ] Remaining deprecated names or modules are called out explicitly.

---

## Rollback: when and how

Trigger rollback when:

- the new path produces unexpected errors, wrong outputs, or missing side effects
- equivalence breaks in production before full observation period completes
- a downstream consumer fails in a way not covered by pre-cutover evidence

To roll back:

- revert callers to the legacy entry point
- confirm the legacy path is producing expected results
- document what failed and why
- do not re-attempt cutover until the failure is explained and the contract is re-verified
