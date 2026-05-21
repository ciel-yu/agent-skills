# DESIGN.md Implementation Workflow

## 1. Read DESIGN.md

Inspect:

- YAML token values
- Overview for tone
- Colors for hierarchy and interaction rules
- Typography for scale
- Layout, Shapes, and Components for density and patterns
- Do's and Don'ts

Tokens are exact. Prose sets usage rules.

## 2. Locate the styling surface

Use the existing system:

- CSS variables or global CSS
- Tailwind config or `@theme`
- component-local styles
- design-system package tokens
- framework theme provider

Do not add a parallel styling system.

## 3. Map tokens

Map only what the task needs:

- colors to variables/theme keys
- typography to heading/body/label styles
- spacing to layout gaps and padding
- rounded values to surfaces and controls
- component tokens to component variants/states

## 4. Compose from rules

Use rationale for ambiguous choices:

- If the identity is editorial, emphasize type rhythm and whitespace.
- If it is playful, allow stronger color and motion.
- If it is utilitarian, prioritize clarity, density, and contrast.
- If it is luxury/refined, use restraint, texture, and precise spacing.

## 5. Verify UI

Use the real surface:

- open the page in a browser
- inspect responsive states when relevant
- trigger hover/active/open states when changed
- watch console output
- compare obvious token values against rendered styles when possible
