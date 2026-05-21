# DESIGN.md Authoring Workflow

## 1. Gather constraints

Ask only for constraints that affect token values:

- product name
- audience
- UI surface
- tone: editorial, playful, technical, luxury, utilitarian, organic, brutalist
- required colors or fonts
- accessibility constraints
- references to match or avoid

If detail is sparse, choose a direction and state it in `## Overview`.

## 2. Write thesis

Use one sentence with mood, hierarchy, and surface quality.

Good:

> Archival editorial calm with precise data-tool ergonomics: warm paper surfaces, ink-heavy headings, and one copper action color.

Weak:

> A modern and clean interface with nice colors.

## 3. Define tokens before prose

Choose exact values:

- colors: hex strings
- typography: family, size, weight, line height, tracking when needed
- spacing scale
- radius scale
- component tokens and states

Use references in component tokens: `{colors.primary}`.

## 4. Write rules

For each section, state usage rules:

- which color drives primary actions
- which type token is for page titles vs labels
- where spacing should feel dense or generous
- which shapes belong to interactive controls vs containers
- what to avoid

Keep rationale operational.

## 5. Validate

Run:

```bash
npx @google/design.md lint DESIGN.md
```

Fix errors. Review warnings for missing typography, broken references, low contrast, and unused colors.
