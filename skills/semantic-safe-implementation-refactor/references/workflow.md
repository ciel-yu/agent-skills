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

## 2. Detect intent and state the preservation boundary

Before planning the replacement, infer intent from evidence:

1. explicit user constraints
2. tests and fixtures
3. public contracts
4. call sites and downstream usage
5. docs and ADRs
6. comments and naming
7. implementation details

Then state which layers must remain stable:

- behavior
- contract
- domain meaning
- names

If confidence is low or the evidence conflicts, stop and ask instead of guessing.

---

## 3. Extract the semantic contract

Load [semantic-contract.md](./semantic-contract.md) and [legacy-debt-handling.md](./legacy-debt-handling.md).

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

Classify every notable legacy behavior as **Intentional**, **Relied-upon accidental**, **Ambiguous**, or **Dead** before porting it.

If you cannot state the contract and classify the key behaviors, you are not ready to rewrite the implementation.

---

## 4. Choose a replacement seam

Load [seam-selection.md](./seam-selection.md) for seam choice.

Prefer a seam that allows new code to live mostly outside the legacy implementation.
Default to a new module behind the same interface. Escalate to façade, adapter, branch-by-abstraction, strangler path, or feature flag only when the migration conditions require it.

Avoid editing inside tangled legacy internals unless no clean seam exists.

---

## 5. Open the ADR gate if needed

Stop and document the decision before implementation if the rewrite changes:

- public names or externally visible behavior
- API, schema, event, or integration contracts
- data meaning or lifecycle semantics
- ownership or bounded-context boundaries
- persistence strategy or architectural layering in a lasting way

If none of those change, continue as semantic-preserving replacement work.

---

## 6. Implement the new path first

In `replace` mode:

1. Create the new module or isolated path.
2. Port behavior according to the semantic contract, not the old control flow.
3. Add tests or fixtures against the contract.
4. Add the smallest possible bridge in legacy call sites.
5. Keep the old path available until equivalence is shown when the blast radius is non-trivial.

Prefer parallel implementation plus cutover over invasive surgery.

---

## 7. Verify equivalence

Load [equivalence-checks.md](./equivalence-checks.md) for evidence selection.

Match checks to the preserved layers first, then choose one or more:

- existing tests that still pass
- new characterization tests
- fixture-based before/after comparisons
- snapshot or golden-output comparison
- API or event transcript comparison
- manual scenario matrix when automation is impossible

Record which preserved layers each check covers.
Do not say "equivalent" without evidence.

---

## 8. Cut over and document residue

Load [cutover-checklist.md](./cutover-checklist.md) and follow it in order.

Do not switch traffic until the pre-cutover checklist is complete.

After equivalence is credible:

- switch callers to the new path
- verify under live or representative conditions
- deprecate or remove the old path when stable
- record deprecated terms, modules, and adapters still present
- document debt carry-forwards

If the migration is partial, say so clearly.
