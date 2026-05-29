---
name: semantic-safe-refactor
description: "Use when the user wants to replace legacy code with a clean implementation without changing behavior, public contracts, or domain meaning. Triggers: rewrite a messy module, refactor without hacking old code, reimplement a service behind the same interface, extract a subsystem behind an adapter, or migrate callers to a new path while preserving semantics. Define a semantic contract first; open an ADR only if the rewrite intentionally changes architecture or externally visible semantics. Do not use for tiny local bug fixes, speculative rewrites with no contract baseline, or redesigns that intentionally change behavior."
---

# Semantic-Safe Refactor

This skill is a **Pipeline + Reviewer** for semantic-preserving replacement refactors. Refactor by **replacing implementations, not patching them in place**: extract the old code's semantic contract, choose a replacement seam, implement a new module or path, and prove equivalence before cutover. Use ADRs only when the rewrite intentionally changes architecture or externally visible semantics.

---

## Use When

Use when the user asks to:

- refactor a messy subsystem without changing what it means or does externally
- replace legacy code with a new module while preserving behavior and domain semantics
- extract a cleaner implementation behind an existing interface, route, schema, event, or API contract
- rewrite a component, service, handler, or workflow instead of layering more hacks onto old code
- migrate from one implementation shape to another with adapters, shims, or staged cutover
- implement new code from the semantics of existing code rather than copying its structure

Do **not** use for:

- tiny local fixes where a small edit is clearly safer than replacement
- speculative rewrites when the existing semantic contract has not been established
- redesigns that intentionally change product behavior, domain meaning, or public contracts without an explicit decision
- style-only cleanup or mechanical formatting changes

---

## Operating Modes

- **`audit`** - read the current implementation, extract the semantic contract, find refactor seams, and propose a replacement plan without editing files
- **`replace`** - implement the new module or path, add the minimum bridging changes needed for cutover, and update ADRs only if the replacement changes semantics or architecture

If intent is ambiguous, default to **`audit`**.

---

## Pipeline

`Intake → Detect Intent & Preservation Boundary → Extract Semantic Contract → Design Replacement Boundary ──[ADR Gate]──▶ Implement New Path → Verify Semantic Equivalence → Cut Over`

- **Intake** - identify the target surface: file, module, subsystem, handler, service, or flow; find existing tests, docs, contracts, and ADRs first.
- **Detect Intent & Preservation Boundary** - infer user intent, artifact intent, and system intent from evidence. State which layers must remain stable: behavior, contract, domain meaning, and public names.
- **Extract Semantic Contract** - map inputs, outputs, invariants, side effects, errors, naming, public/internal boundaries, and observable behavior.
- **Design Replacement Boundary** - choose the smallest clean seam: new module behind the same interface by default, then façade, adapter, branch-by-abstraction, strangler path, or feature flag when conditions require them. Prefer clean replacement over edits inside legacy internals.
- **ADR Gate** - if the refactor changes externally visible semantics, shared boundaries, architecture, data meaning, or long-lived contracts, stop. In `audit`, report the ADR requirement. In `replace`, **DO NOT** continue until the decision is recorded.
- **Implement New Path** - write the replacement code in a new module or isolated path first; keep old code touched only where needed to route traffic or bridge dependencies.
- **Verify Semantic Equivalence** - prove the new path preserves the contract using tests, fixtures, examples, snapshots, or before/after comparisons.
- **Cut Over** - switch callers to the new path, deprecate legacy entry points when safe, and leave clear migration notes where old names or modules still exist.

Do **not** default to in-place mutation of tangled code when a replacement seam can be introduced.

---

## Execution

Load on demand:

1. **During `Detect Intent & Preservation Boundary`:** [references/intent-detection.md](./references/intent-detection.md)
2. **During `Extract Semantic Contract`:** [references/semantic-contract.md](./references/semantic-contract.md) and [references/legacy-debt-handling.md](./references/legacy-debt-handling.md)
3. **During `Design Replacement Boundary`:** [references/seam-selection.md](./references/seam-selection.md)
4. **During cutover planning:** [references/workflow.md](./references/workflow.md)
5. **During `Verify Semantic Equivalence`:** [references/equivalence-checks.md](./references/equivalence-checks.md)
6. **Only if `ADR Gate` opens:** [references/adr-gate.md](./references/adr-gate.md)

Read the existing implementation before proposing a rewrite. Prefer the smallest clean seam that lets the new implementation stand on its own.

---

## Core Rules

- **Semantics first**: preserve externally observable meaning, invariants, and domain intent unless an explicit decision says otherwise.
- **Intent is inferred, not guessed**: prefer user constraints, tests, contracts, and call sites over comments or implementation shape.
- **State the preservation boundary**: say which layers must remain stable—behavior, contract, domain meaning, and names—before writing replacement code.
- **Replace, do not accrete**: prefer a new module or isolated path over more branches in legacy code.
- **Contract before code**: extract the semantic contract before choosing abstractions or file layout.
- **Bridge minimally**: touch legacy code only to create seams, adapters, or cutover points.
- **Keep names stable**: preserve canonical terms and public names unless the change is deliberate and documented.
- **Prove equivalence**: use tests or concrete before/after evidence; inspection alone is not enough.
- **Escalate intentional change**: if semantics, boundaries, or architecture change, open or update an ADR.
- **Expose residue**: call out remaining legacy paths, deprecated modules, and partial migrations.

---

## Deliverables

### `audit`

Return:

- detected intent with confidence and evidence sources
- stated preservation boundary
- current semantic contract
- replacement seam recommendation
- risk list and unknowns
- equivalence-check plan
- ADR requirement if the change is not purely semantic-preserving

Do **not** edit files.

### `replace`

Deliver:

- new or rewritten module(s) implementing the preserved contract
- minimal cutover or adapter edits in legacy call sites
- tests or other evidence for semantic equivalence, mapped to the preserved layers
- ADR update only when semantics or architecture intentionally changed
- migration notes for deprecated paths or names

---

## Verification

- [ ] Existing behavior, interfaces, and constraints were read before redesigning internals.
- [ ] Intent was inferred from evidence in priority order, not guessed from code shape alone.
- [ ] The preservation boundary was stated before implementation.
- [ ] A semantic contract was written down or made explicit before implementation.
- [ ] The replacement was implemented in a new or isolated path where feasible.
- [ ] Legacy code was edited only for seam creation, bridging, or cutover.
- [ ] Semantic equivalence was checked with evidence mapped to the preserved layers.
- [ ] Intentional semantic or architectural changes triggered the ADR gate.
- [ ] Remaining legacy paths or deprecated names are called out explicitly.

---

## Final Standard

Result: the agent can replace brittle legacy implementation with cleaner code while preserving what the system means and how it behaves, instead of deepening the hack.
