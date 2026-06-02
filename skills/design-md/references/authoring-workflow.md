# DESIGN.md Authoring Workflow

Use this when creating a new `DESIGN.md` from product constraints, screenshots, notes, or brand rules.

## 1. Collect only design-changing constraints

Ask for missing context only when it can change token values:

- product name
- target users and primary UI surface
- desired tone: editorial, playful, technical, luxury, utilitarian, organic, brutalist
- required colors, fonts, logos, or brand references
- accessibility requirements
- references to match or avoid

If input is sparse, pick a coherent direction and state the assumption in `## Overview`.

## 2. Write the visual thesis

Start `## Overview` with one sentence covering mood, hierarchy, and surface quality.

Good:

> Archival editorial calm with precise data-tool ergonomics: warm paper surfaces, ink-heavy headings, and one copper action color.

Weak:

> A modern and clean interface with nice colors.

## 3. Choose exact tokens first

Define portable values before writing usage prose:

- colors as hex strings
- typography with family, size, weight, line height, and tracking when relevant
- spacing scale
- radius scale
- component tokens and states

Use token references in component tokens, for example `{colors.primary}`.

## 4. Add usage rules

For each section, say where tokens apply and what to avoid:

- which color owns primary actions
- which type token is for page titles, body text, labels, and metadata
- where spacing should feel dense or generous
- which radii belong to controls versus containers
- which component states need distinct treatment

Keep rationale operational enough for implementation.

## 5. Validate

Run:

```bash
npx @google/design.md lint DESIGN.md
```

Fix errors. Review warnings for missing typography, broken references, low contrast, and unused colors.
