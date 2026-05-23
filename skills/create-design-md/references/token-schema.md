# Token Schema Checklist

## Front matter

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

## Valid token types

- Color: `#` plus sRGB hex, for example `"#B8422E"`
- Dimension: number plus unit, for example `48px`, `1rem`, `-0.02em`
- Spacing: dimension or number
- Token reference: `{path.to.token}`
- Typography object: `fontFamily`, `fontSize`, `fontWeight`, `lineHeight`, `letterSpacing`, `fontFeature`, `fontVariation`

## Component properties

Valid properties:

- `backgroundColor`
- `textColor`
- `typography`
- `rounded`
- `padding`
- `size`
- `height`
- `width`

Represent states as related entries, such as `button-primary-hover`.

## Authoring checks

- Include `primary` when colors exist.
- Include typography when colors exist.
- Use token references in components instead of duplicating raw values.
- Avoid duplicate markdown section headings.
- Preserve useful unknown sections.
- Do not invent token syntax.
