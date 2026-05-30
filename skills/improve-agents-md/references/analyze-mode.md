# Analyze Mode

Use when the user wants discussion, critique, diagnosis, or redesign without edits.

## Workflow

### Phase 1: Inspect

Read:

- the current root `CLAUDE.md` and/or `AGENTS.md`
- nested instruction files if they affect structure
- repository layout and major build/test entrypoints

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
8. **Risks / Tradeoffs**

Do **not** edit files in this mode.
