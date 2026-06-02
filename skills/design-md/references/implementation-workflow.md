# DESIGN.md Implementation Workflow

Use this when changing UI code to follow an existing `DESIGN.md`.

## 1. Read the contract

Inspect:

- YAML token values
- Overview for tone and hierarchy
- Colors for roles and interaction rules
- Typography for scale and text roles
- Layout, Shapes, and Components for density and patterns
- Do's and Don'ts for constraints

Tokens are exact values. Prose explains when to use them.

## 2. Find the styling surface

Use the project's current system:

- CSS variables or global CSS
- Tailwind config or `@theme`
- component-local styles
- design-system package tokens
- framework theme provider

Do not add a parallel styling system.

## 3. Map only needed tokens

Map the tokens required by the task:

- colors to variables or theme keys
- typography to heading, body, label, and metadata styles
- spacing to gaps, margins, and padding
- rounded values to surfaces and controls
- component tokens to variants and states

Preserve exact values and references.

## 4. Resolve ambiguous choices from rationale

Use the stated identity to choose between valid mappings:

- Editorial: emphasize type rhythm and whitespace.
- Playful: allow stronger color and motion.
- Utilitarian: prioritize clarity, density, and contrast.
- Luxury or refined: use restraint, texture, and precise spacing.

## 5. Verify on the real UI

When possible:

- open the changed page in a browser or app shell
- inspect relevant responsive states
- trigger changed hover, active, disabled, open, or focus states
- check console or runtime output
- compare obvious rendered values against `DESIGN.md` tokens
