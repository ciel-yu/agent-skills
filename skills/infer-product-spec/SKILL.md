---
name: infer-product-spec
description: 'Use when the user wants to reconstruct a current-state Product Spec from an existing codebase. Audits routes, screens, handlers, copy, data models, tests, and docs to infer shipped behavior, then writes an evidence-backed Product Spec with explicit open questions. Triggers: user asks what a product currently does, wants a PRD/spec from source code, or needs reverse-engineered requirements before refactor or rewrite. Do NOT use for: greenfield product ideation, pure architecture documentation, or polishing an already-authored PRD.'
---

# Infer Product Spec

Reconstruct a current-state product spec from source code.

Work evidence-first. Do not invent strategy, roadmap, or UX intent that the code cannot support.

---

## Use When

- User wants a product spec / PRD inferred from an existing repository
- User asks what a shipped product currently does
- User needs reverse-engineered requirements before refactor, migration, or rewrite
- User has source code but incomplete or missing product documentation

Do **not** use for:

- Greenfield product ideation
- Pure technical architecture writeups with no product-behavior goal
- Editing an already-complete product spec for wording only

---

## Pipeline

`Source Audit → Feature Reconstruction ──[Evidence Gate]──▶ Gap Review → Generate Document`

- **Source Audit**: inspect user-visible surfaces, route maps, handlers, copy, schemas, tests, and docs
- **Feature Reconstruction**: turn evidence into features, roles, entities, flows, and integrations
- **Evidence Gate**: do not generate until the conditions in [references/sufficiency.md](references/sufficiency.md) are met
- **Gap Review**: mark missing intent as open questions instead of guessing
- **Generate Document**: output with evidence notes using the template; save as `<project-name>-Inferred-Product-Spec.md`

---

## Execution

Load on demand:

1. **Extraction workflow**: [references/extraction-workflow.md](references/extraction-workflow.md) — load first
2. **Source map**: [references/source-map.md](references/source-map.md) — load when locating evidence in unfamiliar stacks
3. **Inference signal catalog**: [reference.md](reference.md) — load when mapping code evidence to product-level meaning
4. **Sufficiency criteria**: [references/sufficiency.md](references/sufficiency.md) — load before generation
5. **Output template**: [templates/inferred-product-spec-template.md](templates/inferred-product-spec-template.md) — load when writing the document

---

## Core Rules

- **Evidence before inference**: prefer code, copy, tests, routes, and docs over intuition
- **Separate certainty levels**: label statements as `Observed`, `Inferred`, or `Unknown`
- **Infer only one step beyond evidence**: if the repo shows behavior but not intent, describe behavior and log the missing intent as an open question
- **Prefer shipped surfaces**: routes, screens, commands, APIs, and tests beat helper names or unused abstractions
- **Ignore dead or dormant paths unless clearly live**: mark feature-flagged, deprecated, admin-only, or test-only behavior explicitly
- **AI is evidence-bound**: only claim AI capability when code, integrations, prompts, SDKs, or UI copy support it
- **Current-state language only**: describe what the product appears to ship now, not what it "should" become

---

## Deliverables

- `<project-name>-Inferred-Product-Spec.md`: a current-state product spec reconstructed from source code, with evidence notes and open questions

---

## Verification

- [ ] Product summary is backed by concrete repository evidence
- [ ] Meaningful shipped features are enumerated, or the document explicitly states why fewer exist
- [ ] At least one end-to-end user flow is reconstructed from source paths
- [ ] Every speculative claim is labeled `Inferred` or moved to `Open Questions`
- [ ] User-visible surfaces, entities, and integrations are named explicitly
- [ ] AI capability appears only when supported by code or UI evidence
- [ ] Output file named `<project-name>-Inferred-Product-Spec.md`

---

## Final Standard

A PM or engineer can read the generated spec and understand what the product currently ships, what evidence supports that reading, and which business-intent gaps still require a human answer.
