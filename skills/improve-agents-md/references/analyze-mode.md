# Analyze Mode

Use when the user wants discussion, critique, diagnosis, or redesign without edits.

## Workflow

### Phase 1: Inspect

Read:

- the current root `CLAUDE.md` and/or `AGENTS.md`
- **all nested `AGENTS.md` files** — map their paths and directory depths
- repository layout and major build/test entrypoints

For monorepos: note whether each project has its own `AGENTS.md` and whether the root file is trying to inline project-specific detail.

### Phase 2: Diagnose

Identify:

- what belongs in the root file
- whether the root file is incorrectly storing current work state instead of durable instructions
- where competing historical patterns require current-direction rules
- what dangerous zones or irreversible operations exist and whether they are captured
- what critical facts (non-obvious invariants, external contracts) are missing from root
- what is too narrow or too long
- what should move out
- what should be conditionalized
- what should be enforced by tooling instead

For monorepos, additionally check:

- Is root acting as a project-detail aggregator instead of a monorepo contract?
- Do project-level files duplicate root content?
- Are global danger zones in root and project-local danger zones in project files?
- Does root describe the repo topology (where projects live) so agents can navigate?
- Are project-level overrides of root conventions explicitly flagged?

### Phase 3: Propose

Return:

- a target top-level structure
- what active status or progress content should be replaced with pointers to authoritative status sources
- whether `Current Direction` is needed and what decision rules it should contain
- what belongs in `Boundaries` (dangerous zones, irreversible operations, critical facts)
- recommended companion docs
- suggested conditional blocks
- anti-patterns to remove

## Output Contract

Include:

1. **Current Problems**
2. **Keep / Move / Conditionalize / Delete**
3. **Recommended Structure**
4. **Status vs Durable Guidance** — what should stop living in root, and where root should point instead
5. **Boundaries** — dangerous zones and critical facts that must stay in root; flag any that are currently missing or buried in companion docs
6. **Current Direction / Decision Rules** when competing patterns conflict
7. **Suggested Companion Docs**
8. **Layer Analysis** *(monorepos only)* — map of all `AGENTS.md` files, duplication across layers, what should move from root to project files or vice versa, missing project-level files
9. **Risks / Tradeoffs**

Do **not** edit files in this mode.
