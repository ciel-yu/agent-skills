---
name: extract-design-md
description: 'Use when extracting DESIGN.md from an existing project. Audits styles, resources, components, and screens, then writes YAML tokens plus short rationale.'
---

# Extract `DESIGN.md`

Create DESIGN.md from shipped UI code.

Use evidence first:

- design resources
- theme files
- component styles
- rendered screens
- repeated values

Do not invent a new visual system.

---

## Use When

Use for requests to:

- extract `DESIGN.md` from an existing app
- document current UI tokens
- reverse-engineer colors, typography, spacing, radii, or components
- consolidate scattered theme values into DESIGN.md
- prepare a design contract before UI refactor work

Do not use when the user wants a new brand direction rather than current-state extraction.

---

## Execution

Load as needed:

1. [Extraction workflow](./references/extraction-workflow.md)
2. [Source map](./references/source-map.md)
3. [Normalization rules](./references/normalization-rules.md)

Write `DESIGN.md` only after auditing source values. Mark inferred rationale as inferred.

---

## Output

Produce:

- `DESIGN.md` with valid front matter
- token source notes in markdown prose
- unresolved questions for missing brand intent
- validation result from `npx @google/design.md lint DESIGN.md` when available

---

## Quality Bar

Required:

- tokens match existing code values
- repeated values become named tokens
- one-off values stay out unless visually important
- component tokens cover common controls
- prose separates observed facts from inferred intent
- no redesign unless the user asks
