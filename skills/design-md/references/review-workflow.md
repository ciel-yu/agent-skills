# DESIGN.md Review Workflow

Use this when validating, linting, comparing, or auditing `DESIGN.md`.

## 1. Check structure

Verify:

- YAML front matter starts the file
- token values use valid shapes
- markdown body uses `##` sections
- section headings are not duplicated
- sections follow canonical order when practical

## 2. Run CLI checks

Run:

```bash
npx @google/design.md lint DESIGN.md
```

Map common findings:

- `broken-ref`: fix unresolved token references
- `missing-primary`: add a primary color or justify its absence
- `contrast-ratio`: inspect component foreground/background pairs
- `orphaned-tokens`: reference unused colors or remove them
- `missing-sections`: add required scales or rationale
- `missing-typography`: add typography tokens
- `section-order`: reorder sections

## 3. Review prose quality

Check for:

- a clear visual thesis
- color roles and hierarchy
- typography roles
- layout density and rhythm
- shapes and component behavior
- concrete Do's and Don'ts

Flag prose that is generic, contradictory, or impossible to implement.

## 4. Compare versions

Use:

```bash
npx @google/design.md diff before.md after.md
```

Flag removed tokens, changed interaction colors, worse contrast, and new warnings.
