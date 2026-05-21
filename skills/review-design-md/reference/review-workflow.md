# DESIGN.md Review Workflow

## 1. Validate structure

Check:

- YAML front matter at the top
- valid token values
- markdown body with `##` sections
- no duplicate section headings
- canonical section order

## 2. Run CLI checks

Run:

```bash
npx @google/design.md lint DESIGN.md
```

Map findings:

- `broken-ref`: must fix
- `missing-primary`: add or justify no primary color
- `contrast-ratio`: check component foreground/background pairs
- `orphaned-tokens`: reference or remove unused colors
- `missing-sections`: add needed scales
- `missing-typography`: add typography tokens
- `section-order`: reorder sections

## 3. Review prose quality

Check for:

- visual thesis
- color roles and hierarchy
- typography roles
- layout density and rhythm
- shapes and component behavior
- concrete do's and don'ts

Flag generic, contradictory, or non-actionable prose.

## 4. Compare versions

Use:

```bash
npx @google/design.md diff before.md after.md
```

Flag removed tokens, changed interaction colors, worse contrast, and new warnings.
