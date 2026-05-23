---
name: review-design-md
description: 'Use when reviewing, linting, validating, or diffing DESIGN.md files for structure, token references, contrast, and regressions.'
---

# Review `DESIGN.md`

Review DESIGN.md for valid tokens, usable rationale, section order, contrast, and regression risk.

---

## Use When

Use for requests to:

- validate or lint a `DESIGN.md`
- review DESIGN.md before implementation
- compare two DESIGN.md versions
- check broken references, contrast, or token regressions
- improve an existing DESIGN.md without changing product UI

Do not use for general UI code review unless DESIGN.md is central.

---

## Execution

Load as needed:

1. [Review workflow](./references/review-workflow.md)
2. [Quality rubric](./references/quality-rubric.md)

Lint:

```bash
npx @google/design.md lint DESIGN.md
```

Diff:

```bash
npx @google/design.md diff DESIGN-old.md DESIGN.md
```

---

## Review Standard

Findings first. Severity order:

1. broken references or invalid structure
2. accessibility and contrast risks
3. missing core tokens
4. vague or contradictory rationale
5. section order, duplication, and maintainability issues

If no findings, say so. Note unverified risks.
