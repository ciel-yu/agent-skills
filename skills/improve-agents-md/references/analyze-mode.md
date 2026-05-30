# Analyze Mode

`DO NOT edit any files in this mode.`

## Phase 1: Inspect

- Read root `AGENTS.md` / `CLAUDE.md` and all nested `AGENTS.md` files; map paths and directory depths.
- Read repository layout and major build/test entrypoints.
- Monorepo: note whether each project has its own `AGENTS.md` and whether root is inlining project-specific detail.

## Phase 2: Diagnose

Identify:

- what belongs in root vs companion docs
- work state stored as durable instructions (should be a pointer, not inline content)
- competing historical patterns that need `## Current Direction` rules
- missing or uncaptured dangerous zones and irreversible operations
- missing critical facts (non-obvious invariants, external contracts)
- content that is too narrow, too long, should be conditionalized, or enforced by tooling

Monorepo additions:

- Is root acting as a project-detail aggregator instead of a monorepo contract?
- Do project files duplicate root content?
- Are global danger zones in root and project-local danger zones in project files?
- Does root describe the repo topology so agents can navigate?
- Are project-level overrides of root conventions explicitly flagged?

## Output Contract

Return in this order:

1. **Current Problems**
2. **Keep / Move / Conditionalize / Delete** — per section
3. **Recommended Structure** — target top-level headings
4. **Status vs Durable Guidance** — what should leave root; where root should point instead
5. **Boundaries** — danger zones and critical facts; flag missing or buried items
6. **Current Direction / Decision Rules** — when competing patterns conflict
7. **Suggested Companion Docs**
8. **Layer Analysis** *(monorepos only)* — `AGENTS.md` file map, cross-layer duplication, what should move between layers, missing project files
9. **Risks / Tradeoffs**
