# agent-skills

Reusable Agent Skills for coding-agent workflows.

## Skills

- `improve-agents-md` - create, review, or rewrite root `AGENTS.md` / `CLAUDE.md` instruction files.
- `extract-design-md` - extract `DESIGN.md` from an existing project's styles, resources, components, and screens.
- `infer-product-spec` - reconstruct a current-state Product Spec from an existing codebase by auditing routes, screens, handlers, copy, models, tests, and docs.
- `create-design-md` - create or substantially rewrite a `DESIGN.md` visual-identity contract.
- `apply-design-md` - implement UI from an existing `DESIGN.md` using project styling conventions.
- `review-design-md` - lint, review, or compare `DESIGN.md` files for structure, contrast, token quality, and regressions.
- `polish-skill-md` - refine, lint, or rewrite an existing `SKILL.md` for triggering, pattern fidelity, and progressive disclosure.
- `semantic-safe-implementation-refactor` - replace brittle implementations behind a materially stable surface and technology model; preserve behavior, contracts, and domain meaning.
- `semantic-safe-module-boundary-migration` - move, split, or consolidate modules while keeping the caller-visible surface materially stable: discovery paths, exports, behavior, errors, and side effects.
- `semantic-safe-technology-migration` - migrate frameworks, runtimes, versions, module systems, or programming models while allowing technical surface change but preserving behavioral and domain semantics.


## References

- https://agentskills.io/specification
- https://agentskills.io/skill-creation/best-practices
- https://agentskills.io/skill-creation/optimizing-descriptions
- https://agentskills.io/skill-creation/evaluating-skills
- https://agentskills.io/skill-creation/using-scripts
- https://github.com/vercel-labs/skills
