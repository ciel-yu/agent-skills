# Methodology

## Core Model

Root `AGENTS.md` is a **cross-session contract** loaded at the start of every session. Write for persistence, not current progress.

| Dimension | Covers |
|-----------|--------|
| **WHY** | Mission, abstract goal, durable objectives |
| **WHAT** | Toolchain or tech stack, repo map, key structural components |
| **DIRECTION** | Canonical patterns, authoritative sources, conflict-resolution rules |
| **HOW** | Default commands, verification flow, workflow constraints |
| **BOUNDARIES** | Danger zones, irreversible operations, critical non-obvious invariants |

In a monorepo, layers accumulate root-first, then project-level. See [layered-setup.md](./layered-setup.md).

**Scope filter:** content that doesn't help most tasks, or needs updating after a normal day of work, does not belong in root. Exception: `## Boundaries` constraints are exempt from length pressure.

---

## Principles

1. **Less is more.** Every root line enters every session and competes for attention. Keep concise; remove low-value detail. Exception: `## Boundaries` constraints are exempt.
2. **Universal over local.** Root serves most tasks. Move feature notes, debugging steps, working state, and style guides to companion docs. Root may point to status; it must not store it.
3. **Clarify current direction.** When the repo has competing conventions, name canonical patterns as `if/then` decision rules. `## Current Direction` holds persistent decisions, not transient state. Heuristic: if a line needs updating after a normal day of work, it is status, not direction.
4. **Abstract goals.** Purpose = mission, system role, durable objectives. Not current progress, implementation details, or milestone tracking.
5. **Progressive disclosure.** Point to companion docs for narrow or deep content. Root says *where to find* active state, not *what the state is*.
6. **Pointers over copies.** File paths and links over copied snippets that drift.
7. **Tooling first.** If a rule can be enforced by a linter, formatter, hook, or test, prefer tooling over instructions.
8. **Conditional guidance.** Narrow rules go in `<important if="...">` blocks with specific conditions, not vague prose. Do not wrap foundational repo identity. Example: `<important if="you are writing or modifying tests">`.
9. **Hand-tuned.** Generate drafts; curate the final file. Every root line affects every session.
10. **Mechanism over state.** Prefer: how to find state, how to resolve conflicts, how to choose canonical patterns. Avoid: "currently implementing X", half-done steps, temporary facts.
11. **No `## Thinking Discipline` heading.** Fold discipline rules into `## Default Workflow`, `## Boundaries`, `## Current Direction`, or `<important if>`. Before adding any rule, it must pass three filters: (a) universal across most tasks, (b) not tool-replaceable, (c) reverses a plausible model default.

---

## `## Current Direction`

Use when the repo has **competing conventions, tools, workflows, or patterns**.

- Concrete `if/then` rules only: what is canonical, what is legacy-only, what is authoritative when sources conflict.
- Decision register, not progress report. Omit abstract Key Principles that don't change decisions.

```markdown
## Current Direction  *(software development)*

- If adding a new API endpoint, use `src/server/routes/`; treat `legacy/api/` as maintenance-only.
- If adding UI, use function components and shared design tokens; do not copy class-component examples.
- If examples conflict, prefer `docs/architecture.md` and newest modules under `src/features/`.
```

```markdown
## Current Direction  *(reverse engineering)*

- If analyzing a new binary, start with static analysis; use dynamic tracing only when static analysis hits a wall.
- If toolchain examples conflict, prefer scripts under `tools/` over ad-hoc one-liners.
- Treat `archive/` as read-only reference; do not copy patterns from it for active targets.
```

```markdown
## Current Direction  *(data mining)*

- For new analysis, use scripts under `pipelines/`; treat `notebooks/explore/` as scratch space, not canonical.
- If a data source has both a v1 and v2 collector, use v2; v1 is maintenance-only.
- When findings need to be shared, output to `reports/`; do not rely on notebook output cells as deliverables.
```

---

## `## Boundaries`

Two categories, both exempt from length pressure:

- **Dangerous zones** — irreversible or high-consequence actions: do-not-modify paths/branches, operations requiring confirmation, environments that must not be touched autonomously.
- **Critical facts** — non-obvious invariants: shared state between systems, hard external contracts, mandatory prerequisites not inferable from the repository.

**Placement rule:** if violating a rule causes irreversible damage, data loss, or unauthorized access → root `## Boundaries`, regardless of scope. Style preferences, single-subsystem warnings, and `"be careful with X"` do not qualify.

```markdown
## Boundaries  *(software development)*

- Do not push directly to `main` or `release/*`; all changes go through pull requests.
- Do not modify `infra/terraform/` without explicit task scope; changes are applied automatically on merge.
- `config/secrets.example` is a template; never commit real credentials.
```

```markdown
## Boundaries  *(reverse engineering)*

- Do not execute untrusted samples on the host machine; use the isolated VM at `vms/analysis-vm`.
- `findings/` is append-only; do not rewrite or delete existing entries.
- Do not upload samples or findings to external services without explicit instruction.
```

```markdown
## Boundaries  *(data mining)*

- `data/raw/` is immutable; never write, modify, or delete files in it.
- Do not run collection scripts against live sources without rate-limit configuration set.
- Do not commit API keys, credentials, or PII to the repository.
```
