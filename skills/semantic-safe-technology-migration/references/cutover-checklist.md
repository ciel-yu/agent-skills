# Cutover Checklist

Use during `Cut Over` after all cohorts are migrated and the observation window has passed.

---

## Pre-cutover

- [ ] Behavioral equivalence is proven for every migrated cohort.
- [ ] All Behavioral-Risk Gate decisions are recorded.
- [ ] No Ambiguous breaking changes remain unresolved.
- [ ] Every compatibility bridge has a named owner and a removal condition.
- [ ] Rollback plan is defined: how to revert if the new tech causes production issues.
- [ ] Monitoring covers the new tech's behavior equivalently to the old.
- [ ] Stakeholders notified if cutover is user-visible or affects shared integrations.

**DO NOT** cut over if behavioral equivalence evidence is incomplete or rollback is undefined.

---

## Cutover

- [ ] Final cohort migrated and verified.
- [ ] Old tech entry points removed or explicitly deprecated.
- [ ] Compatibility bridges that have met their removal condition are removed.
- [ ] CI/CD updated to no longer test old-tech paths where applicable.

---

## Post-cutover

- [ ] Old tech dependency removed from package manifests if fully replaced.
- [ ] Docs, READMEs, and onboarding guides updated to reflect the new tech.
- [ ] Compatibility bridges still in active use are tracked in the bridge registry.
- [ ] Debt carry-forwards are recorded and assigned owners.

---

## Rollback

Trigger rollback when:

- new tech produces behavioral differences not accounted for in equivalence evidence
- a consumer cohort fails in production in a way not covered before cutover
- a compatibility bridge is removed before all its callers have migrated

To roll back:

- revert to the old tech path or re-enable the relevant compatibility layer
- confirm old behavior resumes
- document the failure and root cause before re-attempting cutover
