---
name: improve-agents-md
description: 'Use when creating, reviewing, or rewriting CLAUDE.md or AGENTS.md files. Applies rules: concise onboarding context, current direction, progressive disclosure, pointers over copies, deterministic tools over style prose, and conditional task rules.'
---

# Improve `AGENTS.md`

Create, critique, or rewrite root `CLAUDE.md` / `AGENTS.md` into a **high-leverage onboarding file**, not a prompt dump.

Principles:

- **Entry context**, not knowledge base — every line enters every session.
- Cover **WHY / WHAT / DIRECTION / HOW / BOUNDARIES**.
- Clarify direction and hard boundaries explicitly; keep everything else short.
- Move task-specific guidance to companion docs; prefer pointers over copies.
- Use tooling (linters, formatters, hooks) for deterministic enforcement.

---

## Use When

Use when the user asks to:

- create a new `AGENTS.md`
- improve or rewrite an existing `AGENTS.md`
- convert a large `AGENTS.md` into a layered setup
- make agent instructions shorter, clearer, or more reliable
- extract task-specific guidance from a root instruction file
- clarify current direction when a repo contains competing historical patterns
- discuss instruction-file structure before editing

Do **not** use this skill for ordinary application code changes.

---

## Operating Modes

Modes:

- **`analyze`** - diagnose and redesign without editing files
- **`rewrite`** - implement a refactor of the instruction files

If intent is ambiguous, default to **`analyze`**.

---

## Execution

Load as needed:

1. **Core methodology:** [references/methodology.md](./references/methodology.md)
2. **Analyze mode output contract:** [references/analyze-mode.md](./references/analyze-mode.md)
3. **Rewrite mode workflow:** [references/rewrite-mode.md](./references/rewrite-mode.md)
4. **Quality rubric and anti-patterns:** [references/quality-rubric.md](./references/quality-rubric.md)

Always classify sections before proposing or rewriting.

---

## Final Standard

Result: **reusable operator handbook**, not prompt dump.
