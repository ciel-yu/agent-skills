# DESIGN.md Extraction Workflow

Use this when deriving `DESIGN.md` from an existing product, codebase, theme, or rendered UI. The output documents current state; it is not a redesign.

## 1. Find authoritative style sources

Search for:

- existing `DESIGN.md`
- theme files and global styles
- design-system packages
- component libraries and variant utilities
- XAML or Avalonia resource dictionaries
- CSS variables and Tailwind config
- hard-coded colors, type, spacing, radii, and shadows

Prefer shared theme resources over component-local values.

## 2. Inventory repeated values

Track repeated or named values for:

- colors
- font families, sizes, weights, and line heights
- spacing and layout gaps
- corner radii
- shadows or elevation values
- component states

Keep source paths beside notes during the audit.

## 3. Name tokens by role

Prefer semantic roles over raw value names:

- `primary`
- `secondary`
- `surface`
- `surface-muted`
- `border`
- `danger`
- `body`
- `label`
- `button-primary`

Keep existing project names when they are clear and stable.

## 4. Separate evidence from inference

Write rationale with explicit labels:

- Observed: repeated primary action color in buttons and links.
- Inferred: primary color signals main actions.

Do not claim brand intent that is absent from code, assets, docs, or rendered UI.

## 5. Write DESIGN.md

Create:

- YAML front matter with exact tokens
- `## Overview` with a current-state thesis
- sections for Colors, Typography, Layout, Shapes, and Components
- Do's and Don'ts based on observed patterns

## 6. Validate

Run:

```bash
npx @google/design.md lint DESIGN.md
```

Fix errors. Report warnings that require product or design input.
