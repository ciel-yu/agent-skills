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
## Current Direction
## Default Workflow
## Boundaries
## Additional Context
```

`Current Direction` is optional but recommended when the repo contains competing conventions, tools, or patterns. Use short decision rules for canonical choices, legacy-only areas, and authoritative sources. Omit it when it only restates generic values or duplicates `Boundaries`.

`Current Direction` must hold **persistent decisions**, not live work state. If a line is really progress tracking, implementation status, or a temporary fact, move it out of root and replace it with a pointer to the authoritative status source.

`Boundaries` must capture: dangerous zones (do-not-touch paths, irreversible operations, environments requiring confirmation) and critical facts (non-obvious invariants, hard external contracts the agent cannot infer from code). These are exempt from length pressure — a single concrete boundary line prevents more damage than pages of style guidance.

`Additional Context` should be a short routing section to specific docs.

`Purpose` should stay abstract and durable: mission, objectives, system role. Do not turn it into a snapshot of current implementation progress.

### Phase 3: Rewrite for signal density

Root file should give a map quickly, clarify direction when patterns conflict, define hard boundaries, specify concrete default commands, and route to companion docs instead of inlining detail.

Bias edits toward **mechanism and strategy**:

- how an agent should decide
- how an agent should find deeper or changing information
- where current status lives

Avoid embedding changing work state directly in root.

### Phase 4: Split out detailed guidance

If root contains detailed task-specific content, create companion Markdown files with self-descriptive names.

If root contains active work tracking or current implementation status, move it to companion docs, issue trackers, or handoff artifacts and leave a short pointer in root only when broadly useful.

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
- concrete `if/then` decision rules when patterns conflict
- minimal filler
- standard industry terms
- concise, deterministic, agent-readable phrasing

Avoid:

- motivational language
- vague statements like "write clean code"
- abstract `Key Principles` lists that do not change decisions
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
6. Remove transient status/progress content from root or convert it into a durable pointer
