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
4. **Boundaries** — dangerous zones and critical facts that must stay in root; flag any that are currently missing or buried in companion docs
5. **Current Direction / Decision Rules** when competing patterns conflict
6. **Suggested Companion Docs**
7. **Risks / Tradeoffs**

Do **not** edit files in this mode.
