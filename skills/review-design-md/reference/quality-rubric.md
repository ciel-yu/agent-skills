# DESIGN.md Quality Rubric

## Must pass

- Tokens parse as valid YAML front matter.
- Token references resolve.
- Core colors include a clear `primary` role.
- Typography tokens exist when color tokens exist.
- Components use token references instead of duplicated raw values.
- Markdown sections have no duplicate headings.

## Strong file qualities

- Overview states a clear visual thesis.
- Palette has clear roles, not equal-weight decoration.
- Type scale distinguishes headings, body, labels, and metadata.
- Spacing and radius scales are small and reusable.
- Component tokens cover important repeated controls and states.
- Do's and Don'ts block common misuses.

## Anti-patterns

- prose-only style guides with no exact values
- large token dumps with no rationale
- generic descriptions like "modern, clean, intuitive"
- broken or unused token references
- deferred accessibility
- every color used as an accent
- implementation-specific CSS replacing portable tokens

## Output contract

Return:

- findings with severity and file references
- open questions only when they change the recommendation
- concise suggested fixes
- commands run and high-level results
