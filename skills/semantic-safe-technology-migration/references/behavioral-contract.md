# Behavioral Contract

Extract what must survive the migration before touching any file.

In a tech migration the surface intentionally changes. The behavioral contract defines the only invariant: what the code **does**, not how it is called.

---

## What to capture

| Dimension | Questions |
|---|---|
| Domain purpose | What business or domain problem does this solve? What concept does it represent? |
| Inputs | What data enters? What shape, assumptions, defaults, and validation rules exist? |
| Outputs | What is returned, emitted, stored, or rendered? What does a caller receive? |
| Invariants | What must always remain true before and after execution? |
| Side effects | What is written, sent, logged, cached, or triggered externally? |
| Error behavior | Which errors surface to callers? Which are retried, swallowed, or transformed? What shape do they take? |
| Ordering and sequencing | Does execution order matter? Are there transactional or lifecycle constraints? |
| Performance and lifecycle | Are there SLA guarantees, initialization order requirements, or teardown assumptions callers depend on? |
| User-visible behavior | What does the end user observe? What is invisible to them? |

---

## What is NOT part of the behavioral contract

Do not preserve:

- specific API method names, import paths, or syntax (these are surface; they change)
- internal implementation details not observable by callers or users
- accidental behaviors with no caller reliance (classify as Ambiguous, then decide)
- deprecated patterns being intentionally removed in the target tech

---

## Evidence priority

Prefer evidence in this order:

1. Tests encoding business logic, domain rules, or user scenarios
2. Integration tests, E2E tests, or scenario fixtures
3. API, event, or schema contracts at system boundaries
4. Domain docs, ADRs, product specs
5. Call sites and usage patterns
6. Inline comments and naming intent
7. Implementation details (lowest trust)

If sources disagree, report the disagreement instead of guessing.

---

## Behavioral vs surface classification

For each notable behavior, classify before migrating, then map it to the canonical preservation axis and a verdict:

| Class | Meaning | Canonical class | Default verdict |
|---|---|---|---|
| **Behavioral** | Produces a result or side effect callers depend on; must survive the migration | Required | Preserve |
| **Surface-only** | Caller syntax or API shape changes but outcome is identical | (allowed-to-change surface) | Free-to-change |
| **Ambiguous** | Unclear whether the change is behavioral; flag and resolve before proceeding | Ambiguous | Decision-required |
| **Dead** | No caller evidence of reliance; candidate for removal | Dead | Free-to-change after proof |

Record a **confidence** (high / medium / low) on every behavior. Route any Decision-required item through the behavioral-risk gate before migrating; never upgrade a low-confidence guess to a fact.

---

## Contract template

```md
## Behavioral Contract
- Domain purpose:
- Inputs:
- Outputs:
- Invariants:
- Side effects:
- Error behavior:
- Ordering / lifecycle guarantees:
- Performance / SLA constraints:
- User-visible behavior:
- Per-element class / verdict / confidence:
- Known ambiguities:
```
