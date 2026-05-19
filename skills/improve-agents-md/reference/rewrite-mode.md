# Rewrite Mode

Use this mode when the user wants implementation.

## Workflow

### Phase 1: Inspect current state

Read:

- the current root `CLAUDE.md` and/or `AGENTS.md`
- nested instruction files if they affect structure
- repository layout and major build/test entrypoints

### Phase 2: Decide the target shape

Preferred root structure:

```markdown
# Repository Instructions

## Purpose
## Repository Map
## Default Workflow
## Boundaries
## Additional Context
```

`Additional Context` should be a short routing section to specific docs.

### Phase 3: Rewrite for signal density

Root file should:

- starts with project purpose
- gives a repo map quickly
- specifies concrete default commands where stable
- describes verification expectations
- defines hard boundaries clearly
- routes to extra docs instead of inlining everything

### Phase 4: Split out detailed guidance

If root contains detailed task-specific content, create companion Markdown files with self-descriptive names.

Good candidates:

- testing instructions
- deployment steps
- schema/database rules
- subsystem architecture notes
- language- or package-specific conventions

### Phase 5: Tighten wording

Prefer:

- concrete nouns
- short sections and lists
- observable, actionable instructions
- minimal filler
- standard industry terms
- concise, deterministic, agent-readable phrasing

Avoid:

- motivational language
- vague statements like "write clean code"
- duplicated rules
- stale examples
- filler-heavy or ornate wording

## Delivery Pattern

Rewrite sequence:

1. Update or create root `CLAUDE.md` / `AGENTS.md`
2. Create any companion docs needed for progressive disclosure
3. Add short references in the root file to those docs
4. Preserve any real external contracts or team-specific boundaries
5. Remove redundant or low-signal content
