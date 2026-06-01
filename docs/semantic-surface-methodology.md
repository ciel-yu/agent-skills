# Semantic Surface: Extraction & Delineation Methodology

A tool-agnostic methodology and operating procedure for an AI agent to **extract** the semantic
surface of a unit of code and **delineate** which parts of that surface must be preserved during a
change. It is self-contained and reusable; it does not depend on any particular skill, framework, or
language.

Use it whenever a change must preserve meaning while something about the code moves, is rewritten, or
is upgraded — refactors, module/boundary moves, technology migrations, extractions, deprecations.

---

## 1. Core definition

> The **semantic surface** of a unit is the complete set of observable, meaning-bearing properties
> that anything outside the unit can depend on.

Two words carry the weight:

- **Observable** — visible from *outside* the unit's own internals: to callers, the runtime,
  persistence, downstream systems, or end users. Internal mechanics that no observer can detect are
  not surface.
- **Meaning-bearing** — the property communicates intent or guarantees an outcome. A method name that
  encodes a domain concept is surface; an inlined helper variable name is not.

"Outside the unit" means any of these **observers**:

| Observer | Depends on |
|---|---|
| Callers | how they discover, call, and consume the unit |
| Runtime / loader | discovery paths, init order, lifecycle hooks |
| Persistence | stored shapes, schemas, encodings |
| Downstream systems | events, payloads, protocols, error contracts |
| End users | user-visible behavior and outcomes |
| Integrators / tests | documented contracts and locked behaviors |

The method has exactly **two phases**: **Extract** the full surface (Section 4), then **Delineate**
the preservation boundary across it for the specific task (Section 5). Never merge them — extract
what *exists* before deciding what must *stay*.

---

## 2. The layered surface model

Every surface is described along the same seven layers. This single taxonomy works for any change
type; the change type only affects which layers are preserved (Section 5), not how they are
extracted.

| # | Layer | What it captures | Primary observer |
|---|---|---|---|
| L1 | **Discovery** | How the unit is found: import path, package export, route, registry key, DI token, config name, CLI command | Callers, runtime |
| L2 | **Shape** | Invocation + result contract: exported names, signatures, params, defaults, generics, types/schemas, sync vs async, nullability | Callers, type system |
| L3 | **Behavior** | Output and effect for given input: return values, computed results, branch outcomes | Callers, users |
| L4 | **Effects** | Side effects: writes, network calls, events emitted, logs, cache mutations, resource acquisition/release | Persistence, downstream, runtime |
| L5 | **Failure** | Error semantics: which errors surface, their type/shape/message, retried/swallowed/transformed, partial-failure behavior | Callers, downstream |
| L6 | **Temporal** | Ordering, sequencing, idempotency, transactions, init/teardown order, timing/SLA, lifecycle guarantees | Runtime, callers |
| L7 | **Meaning** | Domain concepts represented, canonical terms, semantically loaded names, ownership, lifecycle stages | Humans, domain |

A unit's semantic surface = the populated set of **surface elements** across L1–L7, each backed by
evidence.

---

## 3. The surface element record

Extraction produces records, not prose. Every element uses the same schema so it can be classified,
gated, and verified later.

```md
- id:            <stable handle, e.g. L2.parse(opts)>
- layer:         L1 Discovery | L2 Shape | L3 Behavior | L4 Effects | L5 Failure | L6 Temporal | L7 Meaning
- observed-by:   <which observer(s) depend on it>
- statement:     <the property, stated as something that is true today>
- evidence:      <source + locator, ranked per Section 6>
- confidence:    high | medium | low
- class:         Required | Relied-on-incidental | Ambiguous | Dead   (assigned in Delineation)
- verdict:       Preserve | Decision-required | Free-to-change         (assigned in Delineation)
```

Rule: **a property is part of the surface only if at least one observer can detect it and at least
one piece of evidence supports it.** No evidence → it is a hypothesis, mark `confidence: low` and
treat as Ambiguous until confirmed or dropped.

---

## 4. Phase A — Extraction procedure

Goal: a complete, evidence-backed surface map of what is observable **today**, independent of the
intended change. Do not decide what to keep yet.

### Step A0 — Frame the unit
State the boundary of the unit under analysis (file, function, module, package, subsystem, flow) and
its outer edge (where "inside" stops and observers begin). Everything crossing that edge is candidate
surface.

### Step A1 — Sweep each layer
Walk L1→L7 in order. For each layer ask the layer's question and record any element found:

- **L1 Discovery** — How does every observer reach this unit? List every path: static import,
  re-export/barrel, dynamic import, route, registry/plugin, DI token, config key, generated binding,
  CLI/command, documented entry point.
- **L2 Shape** — What is the callable/consumable contract? Names, arity, params, defaults, overloads,
  generics, return type, emitted-event shapes, schema, sync/async, nullability.
- **L3 Behavior** — For representative inputs, what output/result is produced? Capture branch
  outcomes that differ observably.
- **L4 Effects** — What does it write, send, emit, log, cache, allocate, or free?
- **L5 Failure** — How does it fail? Error types, messages, codes, which are retried/swallowed/
  transformed/propagated, partial-failure and timeout behavior.
- **L6 Temporal** — What ordering, idempotency, transaction, lifecycle, init/teardown, or timing
  property does any observer rely on?
- **L7 Meaning** — What domain concept does it represent? Which names/terms are canonical and carry
  meaning beyond mechanics?

### Step A2 — Confirm against real observers
Never define the surface from declarations alone. Cross-check each element against actual usage: call
sites, tests/fixtures, runtime wiring, persisted data, downstream consumers, docs that act as
contracts. Usage can **reveal** surface that declarations hide (a caller depending on an incidental
ordering) and can **demote** declared surface that no one uses.

> When an element's true value depends on hidden execution paths — init order across modules,
> exception/retry routes, async sequencing, or a value transformed through several steps before it
> becomes observable — trace that path explicitly. Use control-flow/data-flow tracing as a *lens to
> explain observable behavior*, not as a default formal exercise. A short written path inventory is
> usually enough.

### Step A3 — Attach evidence and confidence
For every element, record its best evidence (Section 6) and a confidence level. Conflicting evidence
is recorded as a conflict, not silently resolved.

**Exit criterion for Phase A:** every observer in Section 1 has been considered, every layer swept,
and every recorded element has evidence + confidence. No `class`/`verdict` is set yet.

---

## 5. Phase B — Delineation procedure

Goal: draw the **preservation boundary** — for each surface element, decide whether the change must
keep it, may change it only with an explicit decision, or is free to change.

### Step B1 — State the preservation intent
Name what the task is fundamentally allowed to change. Three archetypes set different defaults; most
real tasks are one of them:

| Intent archetype | What changes | Default-preserve layers | Layer(s) allowed to change |
|---|---|---|---|
| **Internals stable surface** (rewrite/refactor) | implementation only | L1–L7 all preserved | none (internal mechanics only) |
| **Move the boundary** (module/package/ownership) | location & structure | L2–L7 preserved | L1 Discovery may change *only via compatibility bridge* |
| **Change the technology** (framework/runtime/version) | technical surface | L3, L4, L5, L6, L7 (behavior + meaning) | L1, L2 (discovery + shape) change intentionally |

If the task does not match an archetype, set the boundary per-layer explicitly and justify it.

### Step B2 — Classify each element
Assign `class` to every extracted element:

- **Required** — an in-scope observer depends on it and the intent says it must stay stable.
- **Relied-on incidental** — not designed as a guarantee, but real observers currently depend on it.
  Treat as Required for safety unless a decision retires it.
- **Ambiguous** — evidence is thin or conflicting, or it is unclear whether a change is behavioral.
  Must be resolved before proceeding, not guessed.
- **Dead** — declared/documented but no observer in scope relies on it; candidate for removal.

### Step B3 — Assign a preservation verdict
For each element combine intent (B1) + class (B2):

- **Preserve** — Required or Relied-on-incidental on a default-preserve layer.
- **Decision-required** — the change would alter a preserved layer's observable meaning, a shared/
  published boundary, persisted data meaning, or a long-lived contract; **or** an Ambiguous element
  blocks the change. Stop and get an explicit decision recorded before proceeding.
- **Free-to-change** — on a layer the intent permits to change (e.g. L1 for boundary moves, L1–L2 for
  tech migrations) **or** Dead with confirmed non-reliance.

### Step B4 — Open the gate on intentional change
Any `Decision-required` verdict is a hard stop. In analysis/audit work, report it as a decision the
human must make. In implementing work, do not proceed on that element until the decision exists and
is recorded (e.g. an ADR or equivalent). Surface change that is *expected* by the intent (B1) is not
a gate; only change to a **preserved** layer is.

**Exit criterion for Phase B:** every element has a `class` and `verdict`; every `Decision-required`
item is either resolved or explicitly escalated; the preserved set is the contract the change must
honor.

---

## 6. Evidence priority

When sources disagree, rank trust in this order and **report disagreements instead of guessing**:

1. Explicit human/user constraints for this task
2. Tests and fixtures that encode behavior or domain rules
3. External contracts: API/schema/event/protocol definitions at system boundaries
4. Code at call sites and persistence boundaries (how observers actually use it)
5. Domain docs, ADRs, specs
6. Inline comments and naming intent
7. Implementation details (lowest trust — describes *how*, not *what must hold*)

Higher-ranked evidence overrides lower when they conflict; the conflict is recorded on the element.

---

## 7. Handling ambiguity and confidence

- **Low confidence is a first-class state.** Do not upgrade a guess to a fact to keep moving. An
  Ambiguous element either gets evidence, gets a human decision, or is explicitly carried as a known
  risk.
- **Incidental reliance defaults to preserved.** If a real observer depends on an unintended
  property, keep it until a decision says otherwise — silent removal is the most common
  semantic-break.
- **Conflicting evidence is surfaced, not averaged.** Two tests asserting different behavior is a
  finding, not a tie to break privately.
- **Dead requires proof of non-reliance**, not just absence of obvious callers — check dynamic
  discovery, generated code, and external integrators before removal.

---

## 8. Output templates

### 8.1 Surface map (Phase A output)

```md
## Semantic Surface Map — <unit>
Unit boundary: <what is inside vs outside>
Observers considered: <callers / runtime / persistence / downstream / users / integrators>

| id | layer | observed-by | statement (true today) | evidence | confidence |
|----|-------|-------------|------------------------|----------|------------|
|    |       |             |                        |          |            |
```

### 8.2 Preservation boundary (Phase B output)

```md
## Preservation Boundary — <unit>
Preservation intent: <archetype or explicit per-layer statement>
Default-preserve layers: <…>   Allowed-to-change layers: <…>

| id | layer | class | verdict | rationale / decision ref |
|----|-------|-------|---------|--------------------------|
|    |       |       |         |                          |

Decisions required (gate): <list, or "none">
Residue / known risks: <incidental reliances kept, dead-but-unconfirmed, partial evidence>
```

---

## 9. Worked micro-examples

**Rewrite behind same interface (internals-stable):** `parsePrice(str): Money` is rewritten.
Extraction finds L2 signature, L3 rounding behavior, L5 "throws `InvalidPrice` on empty", L7 term
"price". Intent = internals stable ⇒ all are **Preserve**; equivalence must be proven on L3/L5 for
representative inputs. Only the internal algorithm is Free-to-change.

**Move module to new package (boundary):** `auth/token.ts` → `@core/auth`. L1 import path is
**Free-to-change but via a re-export bridge**; L2–L7 (signatures, behavior, errors, init order,
domain terms) are **Preserve**. A caller relying on load-order side effects (L6) found in A2 is
Relied-on-incidental ⇒ Preserve until decided.

**Express → Fastify (technology):** L1 discovery and L2 handler signatures **change intentionally**
(Free-to-change). L3 response bodies, L4 DB writes, L5 error status codes, L6 middleware ordering, L7
route meaning are **Preserve**. A change to error-body shape is L5 on a preserved layer ⇒
**Decision-required**, even though the framework swap is expected.

---

## 10. Quality checklist

Extraction (Phase A):
- [ ] Unit boundary and observer set stated explicitly.
- [ ] All seven layers swept; each found element has evidence + confidence.
- [ ] Surface confirmed against real observers, not declarations alone.
- [ ] Hidden-path tracing used where observable value depends on it; conflicts recorded.

Delineation (Phase B):
- [ ] Preservation intent named before classifying anything.
- [ ] Every element classified Required / Relied-on-incidental / Ambiguous / Dead.
- [ ] Every element has a Preserve / Decision-required / Free-to-change verdict.
- [ ] Incidental reliance defaulted to Preserve; Dead has proof of non-reliance.
- [ ] Every Decision-required item resolved or escalated through the gate before proceeding.
- [ ] Preserved set captured as the contract for later equivalence verification.

---

## 11. One-paragraph summary for an agent

> Frame the unit and its observers. Extract the full semantic surface across seven layers —
> discovery, shape, behavior, effects, failure, temporal, meaning — backing every property with
> ranked evidence and a confidence level, and confirm it against how observers actually use the unit.
> Then name what the task is allowed to change, classify every element (Required / Relied-on-
> incidental / Ambiguous / Dead), and give each a verdict (Preserve / Decision-required /
> Free-to-change). Stop on any change to a preserved layer or any unresolved ambiguity until a
> decision is recorded. The preserved set is the contract the change must prove it still honors.
