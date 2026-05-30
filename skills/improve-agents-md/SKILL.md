---
name: improve-agents-md
description: 'Use when creating, reviewing, or rewriting CLAUDE.md or AGENTS.md files. Applies rules: cross-session instruction design, concise onboarding context, decision-focused current direction, progressive disclosure, pointers over copies, and strategy/mechanism-oriented guidance.'
---

# Improve `AGENTS.md`

Create, critique, or rewrite root `CLAUDE.md` / `AGENTS.md` into a **high-leverage cross-session operator contract**, not a prompt dump.

Principles:

- **Entry context**, not knowledge base — every line enters every session.
- **Cross-session instruction file**, not status storage — root guidance must persist across sessions and agents.
- Cover **WHY / WHAT / DIRECTION / HOW / BOUNDARIES**.
- Keep overall goals **abstract and durable**: mission, objectives, and decision-worthy intent; not current progress, implementation details, or narrow facts that will drift.
- `## Current Direction` stores **persistent decisions**: canonical choices, authoritative sources, and legacy boundaries. It does **not** store current work state.
- Clarify direction and hard boundaries explicitly; keep everything else short.
- When guidance could be framed either as status or policy, push it toward **mechanism / strategy**: how to find current state, how to choose, how to act.
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
- remove transient work-state content from a root instruction file
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

Always classify sections before proposing or rewriting, including whether any section is incorrectly storing work state instead of durable guidance.

---

## Final Standard

Result: **reusable cross-session operator handbook**, not prompt dump.
