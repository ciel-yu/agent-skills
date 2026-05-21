---
name: create-design-md
description: 'Use when creating or rewriting DESIGN.md. Produces YAML design tokens plus short rationale for colors, typography, spacing, shape, and components.'
---

# Create `DESIGN.md`

Create an agent-readable design contract.

Use two layers:

- YAML front matter: exact tokens.
- Markdown body: token roles and usage rules.

---

## Use When

Use for requests to:

- create a `DESIGN.md`
- define product design tokens
- convert screenshots, notes, or brand rules into tokens
- set colors, typography, spacing, radii, or component tokens before UI work
- convert a style guide to DESIGN.md

Do not use for UI edits unless `DESIGN.md` must change.

---

## Execution

Load as needed:

1. [Authoring workflow](./reference/authoring-workflow.md)
2. [Token schema](./reference/token-schema.md)

Create the file when context is sufficient. Ask only for missing brand constraints that change token choices.

---

## Required Shape

YAML front matter:

- `version`, usually `alpha`
- `name`
- `description` when useful
- `colors`
- `typography`
- `rounded`
- `spacing`
- `components` when repeated UI patterns exist

Markdown `##` section order:

1. Overview
2. Colors
3. Typography
4. Layout
5. Elevation & Depth
6. Shapes
7. Components
8. Do's and Don'ts

---

## Quality Bar

Required:

- exact token values
- clear color roles
- clear type roles
- small spacing and radius scales
- component tokens for repeated controls
- `npx @google/design.md lint DESIGN.md` when available
