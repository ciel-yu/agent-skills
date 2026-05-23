# Normalization Rules

## Colors

- Convert platform color values to hex sRGB.
- Prefer semantic names over raw color names.
- Keep alpha only when translucency is a repeated design pattern.
- Merge near-duplicates only when they render identically or share one resource key.
- Do not rename a clear project token without reason.

## Typography

- Group by role: heading, body, label, caption, code.
- Include family, size, weight, line height when available.
- Infer line height only from rendered styles or explicit platform defaults.
- Do not invent font pairings.

## Spacing

- Extract repeated layout values.
- Use a small scale when values cluster.
- Keep component-specific padding under `components` when it is not a global scale value.
- Preserve platform units when they map cleanly; DESIGN.md accepts dimensions or numbers.

## Radius

- Extract repeated `CornerRadius`, `border-radius`, or theme radius values.
- Name by scale: `sm`, `md`, `lg`, `full`.
- Keep asymmetric radii in component prose unless the schema cannot represent them cleanly.

## Components

- Add component tokens for repeated controls only.
- Use token references instead of raw duplicates.
- Represent states as separate entries: `button-primary-hover`, `button-primary-disabled`.
- Include only valid component properties: `backgroundColor`, `textColor`, `typography`, `rounded`, `padding`, `size`, `height`, `width`.

## Rationale

- Label inferred intent.
- Cite source patterns in prose.
- Use current-state language.
- Avoid redesign language.
