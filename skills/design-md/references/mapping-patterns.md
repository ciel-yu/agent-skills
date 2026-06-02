# Token Mapping Patterns

Use this when translating `DESIGN.md` tokens into project styling systems.

## CSS variables

Map portable token names into existing variable conventions:

```css
:root {
  --color-primary: #1A1C1E;
  --color-accent: #B8422E;
  --radius-sm: 4px;
  --space-md: 16px;
}
```

## Tailwind v4

Generate tokens when possible:

```bash
npx @google/design.md export --format css-tailwind DESIGN.md
```

Merge generated `@theme` values into the existing stylesheet.

## Tailwind v3

```bash
npx @google/design.md export --format json-tailwind DESIGN.md
```

Merge output into `theme.extend`. Do not replace unrelated theme config.

## Component variants

Map component entries to variants or state selectors:

```yaml
components:
  button-primary:
    backgroundColor: "{colors.tertiary}"
  button-primary-hover:
    backgroundColor: "{colors.tertiary-container}"
```

Keep state relationships visible in class names, variants, or selectors.

## Avoid

- hard-coding approximate values when exact tokens exist
- creating a parallel theme
- following prose while ignoring token values
- applying accent colors without hierarchy
