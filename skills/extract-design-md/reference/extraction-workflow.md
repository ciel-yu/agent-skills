# DESIGN.md Extraction Workflow

## 1. Find style sources

Search for:

- theme files
- global styles
- design-system packages
- component libraries
- XAML resource dictionaries
- Avalonia styles
- CSS variables
- Tailwind config
- hard-coded colors, font sizes, spacing, radii

Prefer authoritative theme resources over component-local values.

## 2. Inventory tokens

Record repeated values:

- colors
- font families
- font sizes
- font weights
- line heights
- spacing values
- corner radii
- shadows or elevation values
- component state values

Keep source paths beside notes while auditing.

## 3. Choose token names

Name by role first, value second:

- `primary`
- `secondary`
- `surface`
- `surface-muted`
- `border`
- `danger`
- `body`
- `label`
- `button-primary`

Use existing project names when they are clear and stable.

## 4. Derive rationale

Separate evidence from inference:

- Observed: repeated primary action color in buttons and links.
- Inferred: primary color signals main actions.

Do not claim brand intent that is not present in code, assets, docs, or rendered UI.

## 5. Write DESIGN.md

Create:

- YAML front matter with tokens
- `## Overview` with current-state thesis
- sections for Colors, Typography, Layout, Shapes, Components
- Do's and Don'ts based on observed patterns

## 6. Validate

Run:

```bash
npx @google/design.md lint DESIGN.md
```

Fix errors. Report warnings that need product or design input.
