---
name: improve-agents-md
description: 'Use when creating, reviewing, tightening, or restructuring AGENTS.md or CLAUDE.md repository instruction files. Handles single-project and monorepo repos, including new root-file scaffolding, quality audits, full rewrites, root/nested deduplication, durable-vs-transient guidance, and layered hierarchy design. Default new files to one root AGENTS.md; ask before proposing or creating a layered hierarchy. Do not use for application code, general documentation, other prompt files, or SKILL.md.'
---

# Improve `AGENTS.md`

Create, critique, or rewrite `AGENTS.md` / `CLAUDE.md` into a **cross-session operator contract**, not a prompt dump. This skill is a **Reviewer + Pipeline**: `analyze` mode diagnoses without editing; `rewrite` mode transforms repository evidence and existing instructions into a durable contract.

---

## Use When

Use when the user asks to:

- create a new `AGENTS.md` or `CLAUDE.md` from repository evidence (default to a **single root file**)
- improve, tighten, review, or rewrite an existing one
- design or audit a layered `AGENTS.md` hierarchy in a monorepo or multi-project repo
- decide what belongs in root vs a project-level `AGENTS.md`
- resolve duplication between root and nested instruction files
- convert a large `AGENTS.md` into a layered setup
- extract task-specific guidance from a root instruction file
- remove transient work-state content from a root instruction file
- clarify current direction when a repo has competing historical patterns

Do **not** use for:

- ordinary application code changes
- project documentation that is not an agent instruction file
- polishing skill manifests (`SKILL.md`)

---

## Operating Modes

- **`analyze`** — diagnose and redesign without editing files
- **`rewrite`** — implement a refactor of the instruction files

If intent is ambiguous, default to **`analyze`**.

---

## Pipeline

`inspect → classify ──[MODE GATE]──▶ analyze: report + stop | rewrite: choose layout ──[LAYOUT GATE if layered]──▶ edit → verify`

- **Classification gate:** Classify existing sections or candidate repository facts before proposing text or editing files. Use keep / move / conditionalize / delete for existing content and root / nested / companion / omit for new content.
- **Mode gate:** In `analyze` mode, return findings and stop. In `rewrite` mode, **DO NOT edit files until classification is complete**.
- **Layout gate:** Default new instruction sets to one root file. **DO NOT propose or create a layered hierarchy until the user explicitly approves that direction.** Existing layered setups may be audited without this approval.

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
- **Classify before acting.** Complete classification before drafting, proposing, or editing instruction files.
- **Default to one root file.** For a new setup, create one root file unless the user approves a layered hierarchy.
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
- [ ] `analyze` mode made no edits; `rewrite` mode changed only approved instruction files.

---

## Final Standard

Result: a concise, evidence-based operator handbook that remains useful across sessions.
