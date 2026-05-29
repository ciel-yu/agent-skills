---
name: review-pattern-match
description: 'Use only when the user explicitly specifies the governing development-pattern document for a code review. Supports repo-wide, diff/PR, or selected-file review; extracts explicit rules from the named Markdown guidance, checks code evidence against those rules, and returns severity-ordered findings plus a 0-100 match score with dimension breakdown. Do not use for generic best-practices review, implied standards, style-only linting, or code changes without an explicitly identified governing pattern document.'
---

# Review Pattern Match

Review code against a development-pattern document only when the user explicitly names that governing document, and report how closely the implementation matches it. This skill is a **Reviewer + Inversion + Pipeline**: normalize the named document into reviewable rules, stop and ask when the rules are too abstract to judge, then return evidence-backed findings and a dimensioned match score.

---

## Use When

Use when the user asks to:

- review a repository against a pattern, guideline, or architecture document the user explicitly identifies
- check whether a PR, diff, or patch matches a named development-pattern document
- audit selected files against a specific Markdown conventions document provided or identified by the user
- measure how closely code follows a documented layering, naming, API, or implementation style when the governing document is explicitly in scope
- compare shipped code to a required development pattern before merge or refactor when that pattern document is explicitly specified

Do **not** use for:

- generic code review with no explicitly specified governing pattern document
- cases where the pattern document is only implied from team habits or existing code
- style-only linting, formatting review, or taste-based feedback
- rewriting the pattern document itself
- directly editing code to make it comply

---

## Pipeline

`Intake → Rule Extraction → Evidence Mapping ──[Ambiguity Gate]──▶ Grade Match → Report Findings`

- **Intake**: identify the review surface (`repo`, `diff`, or `selected files`), the governing pattern document, and the requested scope.
- **Rule Extraction**: convert the pattern document into deterministic review rules and separate explicit requirements from soft guidance.
- **Evidence Mapping**: connect extracted rules to concrete code evidence before grading.
- **Ambiguity Gate**: if the document is too abstract, contradictory, or underspecified to support deterministic review, stop and ask the user the minimum clarifying questions in [references/ambiguity-gate.md](./references/ambiguity-gate.md).
- **Grade Match**: score the code using the dimension rubric in [references/scoring-rubric.md](./references/scoring-rubric.md).
- **Report Findings**: return findings first, with file references, pattern linkage, evidence strength, and concise remediation guidance.

Do **not** proceed to grading when the Ambiguity Gate is open.

---

## Execution

Load on demand:

1. **Review workflow and surface-specific steps:** [references/review-workflow.md](./references/review-workflow.md) - load first.
2. **Score model and severity order:** [references/scoring-rubric.md](./references/scoring-rubric.md) - load when grading.
3. **Spec-to-code linkage rules:** [references/evidence-mapping.md](./references/evidence-mapping.md) - load when writing findings.
4. **Clarification gate:** [references/ambiguity-gate.md](./references/ambiguity-gate.md) - load when the pattern document is abstract, contradictory, or mostly aspirational.

---

## Core Rules

- **Document-bound review only**: evaluate code against the specified pattern document, not generic best practices.
- **Explicit document trigger only**: if the user has not clearly identified the governing pattern document, do not activate this skill yet; ask which document should control the review.
- **Architecture before style**: prioritize architecture, code organization, naming/API shape, and idiomatic implementation over formatting or taste.
- **Evidence before judgment**: every finding needs concrete code evidence and a linked pattern rule.
- **Tag evidence strength**: mark each finding as `Exact`, `Section-level`, or `Inferred` per [references/evidence-mapping.md](./references/evidence-mapping.md).
- **Ask instead of guessing**: if a rule cannot be judged deterministically, stop at the Ambiguity Gate and ask.
- **Keep findings terse**: one issue per bullet, concise fix direction, no lecture.
- **Review only**: do not edit code unless the user explicitly asks for a separate implementation step.

---

## Deliverables

Return:

1. **Match Score** - total score `0-100` plus dimension breakdown for `Architecture`, `Code Organization`, `Naming & API Design`, and `Idiomatic Implementation`.
2. **Findings** - severity-ordered findings with file references, rule linkage, evidence strength, and concise remediation.
3. **Coverage Notes** - what surface was reviewed (`repo`, `diff`, or `selected files`) and any excluded areas.
4. **Open Questions** - only questions that would materially change the score or recommendation.

If there are no findings, say so explicitly and still report the score, reviewed surface, and residual uncertainty.

---

## Verification

- [ ] Review surface is named explicitly as `repo`, `diff`, or `selected files`.
- [ ] The governing pattern document is explicitly identified by the user before review begins.
- [ ] Pattern document rules are extracted before findings are written.
- [ ] Every finding includes at least one code reference.
- [ ] Every finding links back to a pattern clause, section, or inferred rule label.
- [ ] Every finding carries an evidence-strength tag: `Exact`, `Section-level`, or `Inferred`.
- [ ] Score includes both total `0-100` and dimension breakdown.
- [ ] Generic best-practices feedback is excluded unless the pattern document itself requires it.
- [ ] If the pattern document is too abstract, the review stops and asks clarifying questions instead of grading.

---

## Final Standard

Result: a reviewer can show exactly which documented development rules the code follows, where it diverges, how strong the evidence is, and how much the implementation matches overall without drifting into subjective code review.
