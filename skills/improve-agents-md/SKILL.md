---
name: improve-agents-md
description: 'Use when creating, reviewing, or rewriting AGENTS.md or CLAUDE.md files — for single-project or monorepo repositories. Covers new file scaffolding (defaults to a single AGENTS.md; ask the user before proposing a layered hierarchy), quality review, full rewrite, layered hierarchy design for monorepos, duplication cleanup between root and project-level files, and converting a flat file into a layered setup. Do not use for ordinary application code changes, non-instruction documentation, or skill manifests (SKILL.md).'
---

# Improve `AGENTS.md`

Create, critique, or rewrite `AGENTS.md` / `CLAUDE.md` into a **cross-session operator contract**, not a prompt dump. This skill is a **Reviewer + Generator + Pipeline**: `analyze` mode diagnoses and proposes without editing files; `rewrite` mode implements; both require a classification step before proceeding.

---

## Use When

Use when the user asks to:

- create a new `AGENTS.md` or `CLAUDE.md` (default to a **single root file**; see Core Rules)
- improve, review, or rewrite an existing one
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

`inspect → classify ──[GATE]──▶ [new file? → layout-decision] → rewrite → verify`

**Gate:** classification (keep / move / conditionalize / delete per section) must complete before any file is touched. In `analyze` mode, the gate is the terminal step — return the plan and stop. In `rewrite` mode, `DO NOT edit files until classification is complete`.

**Layout-decision gate (new files only):** When no `AGENTS.md` exists, default to a **single root file**. Propose a layered hierarchy only when the repo is clearly a monorepo with distinct, divergent projects. When the situation is ambiguous, **ask the user** before creating more than one file.

---

## Execution

Load on demand:

1. **Core model and principles:** [references/methodology.md](./references/methodology.md) — load at the start of every session
2. **Analyze mode workflow and output contract:** [references/analyze-mode.md](./references/analyze-mode.md) — load in `analyze` mode
3. **Rewrite mode workflow:** [references/rewrite-mode.md](./references/rewrite-mode.md) — load in `rewrite` mode
4. **Quality rubric and anti-patterns:** [references/quality-rubric.md](./references/quality-rubric.md) — load during review and before final verification
5. **Layered setup for monorepos:** [references/layered-setup.md](./references/layered-setup.md) — load when the repo is a monorepo or has nested `AGENTS.md` files

---

## Core Rules

- **Classify before acting.** Run section classification before proposing or rewriting. Do not skip this step.
- **Single file by default for new repos.** When no `AGENTS.md` exists, start with one root file. Only escalate to a layered hierarchy if the repo is clearly a monorepo with meaningfully distinct projects. When uncertain, **ask the user** before creating more than one file.
- **Root enters every session.** Every line competes for agent attention; shorter and denser beats longer and comprehensive.
- **Mechanism over state.** Push guidance toward durable operating mechanisms — how to find current state, how to resolve conflicts, how to choose canonical patterns — not temporary fact snapshots.
- **Boundaries are exempt from length pressure.** A single concrete constraint that prevents irreversible harm earns its place in root regardless of specificity.

---

## Final Standard

Result: **reusable cross-session operator handbook**, not prompt dump.
