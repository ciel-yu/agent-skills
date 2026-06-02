# Token Schema Checklist

Use this when authoring or repairing the YAML front matter in `DESIGN.md`.

## Minimal shape

```yaml
---
version: alpha
name: Product Name
description: Short visual identity summary
colors:
  primary: "#1A1C1E"
typography:
  h1:
    fontFamily: Public Sans
    fontSize: 3rem
    fontWeight: 700
    lineHeight: 1
rounded:
  sm: 4px
spacing:
  md: 16px
components:
  button-primary:
    backgroundColor: "{colors.primary}"
    textColor: "{colors.neutral}"
    rounded: "{rounded.sm}"
    padding: 12px
---
```

## Valid token values

- Color: sRGB hex string, for example `"#B8422E"`
- Dimension: number plus unit, for example `48px`, `1rem`, `-0.02em`
- Spacing: dimension or number
- Reference: `{path.to.token}`
- Typography object: `fontFamily`, `fontSize`, `fontWeight`, `lineHeight`, `letterSpacing`, `fontFeature`, `fontVariation`

## Component properties

Use only supported component properties:

- `backgroundColor`
- `textColor`
- `typography`
- `rounded`
- `padding`
- `size`
- `height`
- `width`

Represent states as sibling entries, for example `button-primary-hover` and `button-primary-disabled`.

## Checks

- Include `colors.primary` when any colors exist.
- Include typography tokens when color tokens exist.
- Use token references in components instead of duplicating raw values.
- Avoid duplicate markdown section headings.
- Preserve useful unknown sections instead of deleting them.
- Do not invent token syntax or unsupported component properties.
