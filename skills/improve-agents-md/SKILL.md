---
name: improve-agents-md
description: 'Use when creating, reviewing, rewriting, or splitting AGENTS.md or CLAUDE.md repository instructions. Covers single-project and monorepo files, root/nested deduplication, durable-vs-transient guidance, and layered hierarchy design. For new setups, default to one root AGENTS.md and ask before proposing multiple files. Do not use for application code, general docs, other prompt formats, or SKILL.md.'
---

# Improve `AGENTS.md` / `CLAUDE.md`

Turn repository instruction files into a durable **cross-session operator contract**, not a prompt dump. This skill is a **Reviewer + Pipeline**: `analyze` diagnoses without editing; `rewrite` creates or refactors files from repository evidence.

---

## Use When

Use when the user asks to:

- create, improve, review, or rewrite `AGENTS.md` / `CLAUDE.md`
- separate durable instructions from transient work state
- decide what belongs in root, nested files, or companion docs
- audit duplication or conflicts across instruction-file layers
- design or convert to a layered hierarchy after user approval
- clarify canonical direction when a repo contains competing historical patterns

Do **not** use for:

- ordinary application code changes
- project documentation that is not an agent instruction file
- polishing skill manifests (`SKILL.md`)

---

## Operating Modes

- **`analyze`** — diagnose and redesign without editing files
- **`rewrite`** — create or refactor instruction files

If intent is ambiguous, default to **`analyze`**.

---

## Pipeline

`inspect → classify ──[MODE GATE]──▶ analyze: report + stop | rewrite: plan ──[LAYOUT GATE before new layers]──▶ edit → verify`

- **Classification gate:** Before drafting or editing, classify existing content as keep / move / conditionalize / delete and new evidence as root / nested / companion / omit.
- **Mode gate:** In `analyze`, return findings and stop. In `rewrite`, **DO NOT edit until classification is complete**.
- **Layout gate:** Default new setups to one root file. Before recommending or creating new layers, ask whether the user wants a layered design; proceed only after approval. Existing layers may be audited without approval.

---

## Execution

Load on demand:

1. **Durability and scope model:** [references/methodology.md](./references/methodology.md) — load during classification when separating durable instructions from work state
2. **Analyze workflow and output contract:** [references/analyze-mode.md](./references/analyze-mode.md) — load only in `analyze` mode
3. **Rewrite workflow:** [references/rewrite-mode.md](./references/rewrite-mode.md) — load only in `rewrite` mode
4. **Quality rubric and anti-patterns:** [references/quality-rubric.md](./references/quality-rubric.md) — load during review and final verification
5. **Layered setup:** [references/layered-setup.md](./references/layered-setup.md) — load when nested files already exist or after the user approves a layered proposal

---

## Core Rules

- **Use repository evidence.** Read the instruction files, repository layout, and canonical build/test entrypoints. Do not invent commands, paths, architecture, or policy.
- **Verify loading semantics.** Identify the target agent and confirm how it discovers and prioritizes `AGENTS.md` / `CLAUDE.md` before designing layers.
- **Write for broad loading.** Keep root concise because every line competes for attention across its scope.
- **Prefer mechanism over state.** Explain how to find current state, resolve conflicts, and choose canonical patterns; do not embed temporary progress.
- **Preserve boundaries.** Keep concrete constraints that prevent irreversible harm, even when they are narrow. Never weaken user-authored safety rules.

---

## Deliverables

### `analyze`

- Findings ordered by severity, with keep / move / conditionalize / delete classifications
- A recommended structure and migration plan, with no file edits
- An explicit **no findings** result when the current files already pass; do not invent churn

### `rewrite`

- Updated instruction files based only on verified repository evidence
- A concise summary of moved, removed, conditionalized, and preserved guidance
- Verification results, including any unresolved uncertainty

---

## Verification

- [ ] The target agent's discovery and precedence rules are confirmed, especially for layered files.
- [ ] Every command, path, architecture claim, and policy is supported by repository or user evidence.
- [ ] Root contains broadly applicable guidance; narrow detail is conditionalized, nested, or linked.
- [ ] `Current Direction` contains durable decision rules, not work status.
- [ ] Dangerous operations and critical invariants remain explicit in `## Boundaries`.
- [ ] Companion docs and nested files are linked or discoverable without duplicating parent guidance.
- [ ] `analyze` made no edits; `rewrite` changed only in-scope instruction files.

---

## Final Standard

Result: a concise, evidence-based operator handbook that remains useful across sessions.
