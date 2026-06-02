# Normalization Rules

Use this during extraction to convert platform-specific style values into portable `DESIGN.md` tokens without redesigning them.

## Colors

- Convert platform colors to sRGB hex.
- Prefer semantic role names over raw color names.
- Keep alpha only when translucency is a repeated pattern.
- Merge near-duplicates only when they render identically or share one resource key.
- Do not rename clear project tokens without a reason.

## Typography

- Group type by role: heading, body, label, caption, code.
- Include family, size, weight, and line height when available.
- Infer line height only from rendered styles or explicit platform defaults.
- Do not invent font pairings.

## Spacing

- Extract repeated layout values.
- Use a small scale when values cluster.
- Keep component-specific padding under `components` when it is not a reusable spacing token.
- Preserve platform units when they map cleanly; DESIGN.md accepts dimensions or numbers.

## Radius

- Extract repeated `CornerRadius`, `border-radius`, or theme radius values.
- Name by scale: `sm`, `md`, `lg`, `full`.
- Put asymmetric radii in component prose when the schema cannot represent them cleanly.

## Components

- Add component tokens only for repeated controls.
- Use token references instead of raw duplicates.
- Represent states as separate entries: `button-primary-hover`, `button-primary-disabled`.
- Use only valid component properties: `backgroundColor`, `textColor`, `typography`, `rounded`, `padding`, `size`, `height`, `width`.

## Rationale

- Label inferred intent.
- Cite source patterns in prose.
- Use current-state language.
- Avoid redesign language.
