# Seam Selection

Use this reference during `Design Replacement Boundary`.

Goal: choose the smallest clean seam that lets the new implementation stand on its own, be verified before cutover, and avoid deeper entanglement with legacy internals.

---

## Default path

Prefer this order:

1. **New module behind the same interface**
2. **Façade** when callers should not know old vs new
3. **Adapter** when the legacy call shape differs from the cleaner internal shape
4. **Branch-by-abstraction** when many callers must migrate gradually
5. **Strangler path** when replacing a subsystem, not just a module
6. **Feature flag** only when rollout risk, not design uncertainty, is the main issue

Do not treat these as equal options. Start with the smallest seam that isolates the new code.

---

## Choose by condition

### 1. New module behind the same interface

Use when:

- the public contract stays the same
- one interface or entry point already exists
- callers do not need to change much
- you can prove equivalence at that boundary

This is the default.

### 2. Façade

Use when:

- callers are scattered
- you need one stable entry point over old and new paths
- cutover should happen in one place
- you want to hide migration detail from callers

A façade is usually better than editing many call sites early.

### 3. Adapter

Use when:

- the new module has a cleaner shape than the legacy interface
- old payloads or argument order still need support
- compatibility is required during migration

Use adapters to translate shape, not to hide semantic drift.

### 4. Branch-by-abstraction

Use when:

- migration will happen in stages
- multiple callers must move over time
- rollback is important
- both paths may coexist briefly

Keep the abstraction narrow. Do not let it become a second legacy layer.

### 5. Strangler path

Use when:

- the target is a subsystem or workflow, not one module
- boundaries are coarse and traffic can be redirected gradually
- partial migration is expected for a while

Define ownership and routing rules explicitly.

### 6. Feature flag

Use when:

- rollout risk is high
- you need controlled exposure or fast rollback
- the design is already chosen and the flag is only for release control

Do not use a feature flag as a substitute for a clear seam.

---

## Selection factors

Check these before choosing:

- **Contract stability** - can the public surface stay the same?
- **Caller spread** - is cutover centralized or scattered?
- **Compatibility burden** - do old shapes or names need support?
- **Blast radius** - how much breaks if the cutover fails?
- **Rollback need** - do you need quick reversal?
- **Parallel-run ability** - can old and new paths be compared safely?
- **Boundary quality** - is there already a natural interface, or must one be introduced?

If the seam makes these factors worse, it is probably the wrong seam.

---

## Anti-patterns

Avoid:

- editing deep inside legacy internals when a boundary could be introduced first
- adding many conditional branches to old code instead of isolating a new path
- picking a seam that silently changes the public contract
- dual-running old and new code with no equivalence plan
- using adapters to mask real semantic changes that need an ADR

---

## Minimal seam note template

Use this in planning output:

```md
## Replacement Seam
- Chosen seam:
- Why this seam:
- Boundary introduced or reused:
- Caller impact:
- Rollback path:
- Equivalence check point:
- Residual legacy touchpoints:
```
