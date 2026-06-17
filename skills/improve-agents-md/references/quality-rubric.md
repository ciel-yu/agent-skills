# Quality Rubric

## Rewrite Checklist

- [ ] A new agent can explain what the repo is within 30 seconds.
- [ ] Purpose is a durable mission/objective, not current progress.
- [ ] The file gives a clear first-pass repo map.
- [ ] Default workflow is obvious.
- [ ] `## Current Direction` uses concrete `if/then` rules, not status or milestone tracking.
- [ ] All dangerous zones and irreversible operations are in `## Boundaries`.
- [ ] Task-specific detail is discoverable from root without bloating it.
- [ ] Active work state is not stored in root; root points to it only when needed.
- [ ] Automated rules are not manual style prose.
- [ ] Every section matters across many sessions.

Monorepos additionally:

- [ ] Root describes topology and notes that projects have their own `AGENTS.md`.
- [ ] Root does not inline project-specific stack, workflow, or boundaries.
- [ ] Each project `AGENTS.md` covers only its own scope; no root content repeated.
- [ ] Global boundaries in root; project-local boundaries in project files.
- [ ] Project-level overrides of root conventions are explicitly flagged.

---

## Anti-Patterns

Remove on sight:

- giant shell-command lists with no prioritization
- style-guide sections that belong in tooling
- abstract Key Principles lists that don't change decisions
- current progress, active implementation status, or milestone tracking
- `## Current Direction` that is really a changelog or sprint board
- outdated copied snippets
- instructions for one rare task
- the same warning restated multiple ways (exception: a single concrete Boundaries constraint is not a repeated warning — do not merge or soften it)
- aspirational guidance with no operational meaning (`"be careful with X"`)
- `## Thinking Discipline` or `## Reasoning Framework` heading — fold real content into `## Default Workflow`, `## Boundaries`, `## Current Direction`, or `<important if>` blocks

---

## Final Check

- [ ] Root file is materially shorter or denser.
- [ ] Companion docs are linked from root.
- [ ] `Current Direction` uses concrete decision rules, paths, and legacy boundaries; no status.
- [ ] No dangerous zones or irreversible operations were softened or dropped from `## Boundaries`.
- [ ] No critical fact that prevents silent mistakes was removed.
- [ ] Result reads like a reusable operator handbook.
