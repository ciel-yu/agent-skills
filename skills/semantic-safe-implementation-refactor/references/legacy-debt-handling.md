# Legacy Debt Handling

Use this reference during `Extract Semantic Contract`, alongside [semantic-contract.md](./semantic-contract.md).

Goal: classify legacy behaviors before writing the replacement, so the agent preserves what must survive, flags what is ambiguous, and confidently drops what no one relies on.

---

## Four behavior classes

Classify every notable legacy behavior into one of these before porting it:

| Class | Definition | Action |
|---|---|---|
| **Intentional** | Designed, documented, or tested behavior that callers expect | Preserve |
| **Relied-upon accidental** | Undocumented behavior that callers depend on anyway | Preserve with a note |
| **Ambiguous** | Cannot determine whether it is depended on without more evidence | Flag; ask or verify before porting |
| **Dead** | No caller can observe it; no test covers it | Drop; document the removal |

Never port "dead" behavior silently. Never drop "relied-upon accidental" behavior silently.

---

## How to tell if a behavior is relied on

Strong signals it is relied upon:

- a test asserts it explicitly
- a call site depends on the specific return shape, error type, or side effect
- downstream code handles a quirk that only exists because of this behavior
- a comment says "do not change this"
- it appears in API docs, schema, or an ADR
- removing it breaks something observable

Weak signals only:

- it exists in code
- it looks like it might be important
- it has been there a long time
- no one has complained about it

Weak signals alone are not enough to classify something as relied upon.

---

## The accidental-but-relied-on trap

The most dangerous class is **relied-upon accidental behavior**: behavior that was not designed, may look like a bug, but callers have built on top of it.

Signs you may be in this trap:

- the behavior looks wrong but removing it breaks something
- the behavior is inconsistent across code paths, yet callers handle each variant
- a downstream consumer silently relies on a quirk in error payloads, null returns, or ordering
- tests pass against the quirk, not the ideal behavior

Rules:

- do not fix relied-upon bugs during a semantic-preserving refactor
- preserve the quirk, add a note, and route the fix through a separate deliberate decision
- if you are unsure, mark it ambiguous and ask

---

## What to drop confidently

Drop without an ADR when all of these are true:

- no test covers the behavior
- no call site can observe it
- no persistent state or external event depends on it
- removing it produces no observable difference to any caller

Document what was dropped and why. Do not delete silently.

---

## What always needs an ADR before dropping

Even if the behavior looks dead or accidental, open an ADR when:

- the change affects public API, schema, event, or route semantics
- the change affects error codes or retry contracts that callers may rely on
- the behavior is in a shared boundary between teams or services
- you are not certain about downstream consumers

---

## Debt that cannot be cleaned now

Some debt should not be cleaned in this refactor because it is too risky or too large.

When that is true:

- carry it forward explicitly in the replacement
- add a clearly named comment or module-level note
- record it in the equivalence evidence as a known carry-forward
- do not pretend the new code is cleaner than it is

Leaving debt visible is better than hiding it inside a new module.

---

## Classification template

Use this when building the semantic contract:

```md
## Legacy Behavior Inventory
| Behavior | Class | Evidence | Action | Notes |
|---|---|---|---|---|
| <description> | Intentional / Relied-upon accidental / Ambiguous / Dead | <source> | Preserve / Flag / Drop | |
```

---

## Decision rules at a glance

```
Has a test or explicit caller dependency?
  → Relied-upon. Preserve.

Looks like a bug but something depends on it?
  → Relied-upon accidental. Preserve, add note, route fix separately.

Cannot tell if anyone depends on it?
  → Ambiguous. Ask or verify before porting.

No test, no caller, no observable effect?
  → Dead. Drop and document.
```
