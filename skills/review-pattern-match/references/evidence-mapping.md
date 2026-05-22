# Evidence Mapping

Every finding must connect a code fact to a documented rule.

## Evidence-strength labels

### `Exact`

Use when the finding maps to a direct quote, rule, or unmistakable clause in the pattern document.

Example shape:

- **Pattern**: "Handlers must depend on services, not repositories."
- **Code evidence**: `src/handlers/createUser.ts` imports `userRepository` directly.

### `Section-level`

Use when the document gives a clear section-level expectation but not a single sentence that states the exact rule.

Example shape:

- **Pattern section**: "Layering and responsibilities"
- **Code evidence**: route modules combine HTTP, validation, and data persistence logic.

### `Inferred`

Use only when the document strongly implies a rule but does not state it directly and the conclusion is still useful.

Requirements:

- name it as inferred
- explain the inference in one short clause
- lower confidence accordingly
- do not base the entire review on inferred rules

## Finding template

For each finding, include:

1. **Severity**
2. **Title**
3. **Code** - file reference plus the relevant implementation fact
4. **Pattern** - exact quote, section, or inferred rule label
5. **Evidence strength** - `Exact`, `Section-level`, or `Inferred`
6. **Why it matters** - one sentence
7. **Suggested correction** - one sentence

## Anti-patterns

Do not:

- cite a code location without linking it to the pattern document
- cite a pattern rule without concrete code evidence
- hide inference as certainty
- give multi-issue findings that mix different rules
- use vague pattern linkage like "not aligned with best practices"
