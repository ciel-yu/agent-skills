---
name: apply-design-md
description: 'Use when implementing UI from DESIGN.md. Maps tokens to CSS variables, Tailwind theme values, component variants, and page styles.'
---

# Apply `DESIGN.md`

Implement UI from `DESIGN.md`. Use YAML as source values and markdown as usage rules.

---

## Use When

Use for requests to:

- build or restyle UI from `DESIGN.md`
- convert DESIGN.md tokens into CSS variables or Tailwind theme values
- update components from DESIGN.md tokens
- keep frontend work aligned with DESIGN.md
- explain DESIGN.md implementation choices

Do not use outside UI or styling work.

---

## Execution

Load as needed:

1. [Implementation workflow](./reference/implementation-workflow.md)
2. [Mapping patterns](./reference/mapping-patterns.md)

Read `DESIGN.md` before UI edits. If several exist, use the nearest file unless the user names one.

---

## Core Rules

- Use token values exactly; do not approximate colors, spacing, radii, or font scales.
- Use rationale for hierarchy, density, and interaction states.
- Use the existing styling system.
- Use `npx @google/design.md export` when generated tokens reduce copy errors.
- Avoid token pairs that fail contrast when valid alternatives exist.

---

## Deliverables

Deliver UI changes and note the tokens used.

Verify in the UI surface.
