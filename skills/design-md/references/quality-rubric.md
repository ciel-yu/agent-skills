# DESIGN.md Quality Rubric

Use this during review to classify findings and decide whether a `DESIGN.md` is ready to drive implementation.

## Must pass

- YAML front matter parses.
- Token references resolve.
- Core colors include a clear `primary` role.
- Typography tokens exist when color tokens exist.
- Components use token references instead of duplicated raw values.
- Markdown sections do not duplicate headings.

## Strong file qualities

- Overview states a clear visual thesis.
- Palette has roles, not equal-weight decoration.
- Type scale distinguishes headings, body, labels, and metadata.
- Spacing and radius scales are small and reusable.
- Component tokens cover repeated controls and states.
- Do's and Don'ts prevent common misuse.

## Anti-patterns

- prose-only style guides with no exact values
- large token dumps with no rationale
- generic descriptions such as "modern, clean, intuitive"
- broken or unused token references
- deferred accessibility checks
- every color treated as an accent
- implementation-specific CSS replacing portable tokens

## Output contract

Return:

- findings with severity and file references
- open questions only when they change the recommendation
- concise suggested fixes
- commands run and high-level results
- an explicit no-findings result when nothing blocks readiness
