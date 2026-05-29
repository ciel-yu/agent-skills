# Replacement Workflow

Use this workflow when the task is to replace an implementation while preserving semantics.

---

## 1. Read the existing surface first

Before proposing new abstractions, locate the current semantic surface:

- public APIs, routes, events, schemas, commands, or UI actions
- user-visible copy and domain terms
- tests, fixtures, snapshots, and examples
- module boundaries and call sites
- existing ADRs or design docs that constrain behavior

Do not start from the clean architecture you wish existed. Start from what the system currently promises.

---

## 2. Extract the semantic contract

Write down the contract in concrete terms:

- inputs
- outputs
- invariants
- side effects
- ordering requirements
- error behavior
- idempotency / retry behavior
- timing or consistency expectations when relevant
- canonical domain terms and names that must remain stable

If you cannot state the contract, you are not ready to rewrite the implementation.

---

## 3. Choose a replacement seam

Prefer a seam that allows new code to live mostly outside the legacy implementation.

Common seams:

- new module behind the same interface
- façade in front of old and new implementations
- adapter translating old call shape to new module
- feature-flagged path
- branch-by-abstraction
- strangler path for a subsystem

Avoid editing inside tangled legacy internals unless no clean seam exists.

---

## 4. Open the ADR gate if needed

Stop and document the decision before implementation if the rewrite changes:

- public names or externally visible behavior
- API, schema, event, or integration contracts
- data meaning or lifecycle semantics
- ownership or bounded-context boundaries
- persistence strategy or architectural layering in a lasting way

If none of those change, continue as semantic-preserving replacement work.

---

## 5. Implement the new path first

In `replace` mode:

1. Create the new module or isolated path.
2. Port behavior according to the semantic contract, not the old control flow.
3. Add tests or fixtures against the contract.
4. Add the smallest possible bridge in legacy call sites.
5. Keep the old path available until equivalence is shown when the blast radius is non-trivial.

Prefer parallel implementation plus cutover over invasive surgery.

---

## 6. Verify equivalence

Use one or more:

- existing tests that still pass
- new characterization tests
- fixture-based before/after comparisons
- snapshot or golden-output comparison
- API or event transcript comparison
- manual scenario matrix when automation is impossible

Do not say "equivalent" without evidence.

---

## 7. Cut over and document residue

After equivalence is credible:

- switch callers to the new path
- deprecate or remove the old path when safe
- note residual compatibility shims
- record deprecated terms, modules, or adapters still present

If the migration is partial, say so clearly.
