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
- what is too narrow or too long
- what should move out
- what should be conditionalized
- what should be enforced by tooling instead

### Phase 3: Propose

Return:

- a target top-level structure
- whether `Current Direction` is needed and what decision rules it should contain
- recommended companion docs
- suggested conditional blocks
- anti-patterns to remove

## Output Contract

Include:

1. **Current Problems**
2. **Keep / Move / Conditionalize / Delete**
3. **Recommended Structure**
4. **Current Direction / Decision Rules** when historical patterns conflict
5. **Suggested Companion Docs**
6. **Risks / Tradeoffs**

Do **not** edit files in this mode.
