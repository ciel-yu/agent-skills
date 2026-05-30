# Rewrite Mode

## Phase 1: Inspect

- Read root `AGENTS.md` / `CLAUDE.md` and all nested `AGENTS.md` files; map paths.
- Read repository layout and major build/test entrypoints.
- Monorepo: load [layered-setup.md](./layered-setup.md) if nested `AGENTS.md` files exist or the repo structure warrants it.

## Phase 2: Target shape

Preferred root structure:

```markdown
# Repository Instructions

## Purpose
## Repository Map
## Current Direction
## Default Workflow
## Boundaries
## Additional Context
```

- `Current Direction` — include when repo has competing conventions. Short `if/then` rules for canonical choices, legacy-only areas, and authoritative sources. Must hold **persistent decisions**, not live work state; if a line is progress tracking or a temporary fact, replace it with a pointer.
- `Boundaries` — dangerous zones + critical facts, both exempt from length pressure.
- `Purpose` — abstract and durable: mission, objectives, system role. Not current implementation progress.
- `Additional Context` — short routing section to specific docs.

## Phase 3: Rewrite for signal density

| Prefer | Avoid |
|--------|-------|
| Concrete nouns and observable actions | Motivational language |
| `if/then` decision rules when patterns conflict | Abstract Key Principles lists |
| Mechanism: how to decide, where to find state | Work state embedded in root |
| Companion doc pointers for task-specific detail | Inlined content that will drift |
| Concise, deterministic phrasing | Duplicated rules, stale examples |

## Phase 4: Split out detailed guidance

Move to companion docs:

- testing instructions, deployment steps, schema/database rules
- subsystem architecture notes, language- or package-specific conventions
- active work tracking or implementation status → tracker or handoff artifact; leave a pointer in root only when broadly useful

Monorepo — create or update per-project `AGENTS.md`:

- Move project-specific stack, workflow, and boundaries into the project file.
- Remove that content from root.
- Add a discovery pointer at root: "Each project has its own `AGENTS.md`; read it when working inside that project."
- See [layered-setup.md](./layered-setup.md) for the full rewrite sequence.

## Phase 5: Deliver

Verify before finishing:

- [ ] Root file is materially shorter or denser than before.
- [ ] Companion docs are linked with load cues.
- [ ] `Current Direction` uses concrete decision rules; no progress tracking remains.
- [ ] All dangerous zones and irreversible operations are in `## Boundaries`; none softened or dropped.
- [ ] No critical facts were removed.
- [ ] Transient status/progress has been moved behind pointers.
