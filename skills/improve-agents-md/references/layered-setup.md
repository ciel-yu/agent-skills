# Layered `AGENTS.md` for Monorepos

## When to use a layered setup

Use layered `AGENTS.md` files when a monorepo contains projects with distinct tech stacks, local workflows, or local boundaries that would bloat the root file if inlined. A single flat `AGENTS.md` at root is fine when the repo is small or all projects share the same stack.

---

## Confirm loading semantics first

Instruction-file names, traversal, precedence, and merge behavior differ across agents. Before proposing layers:

1. Identify the target agent or agents.
2. Verify their documented `AGENTS.md` / `CLAUDE.md` discovery rules.
3. Record whether parent and nested files accumulate, override, or load only on demand.
4. Design the hierarchy for the verified behavior; do not assume a root-to-working-directory merge.

When the target agent accumulates parent and nested files, root guidance applies broadly and nested files should add only local, non-duplicated rules. If multiple agents must share the repository, choose the simplest layout they all support or document agent-specific entrypoints.

---

## Hierarchy model

```
<repo>/AGENTS.md                      ← root: monorepo-wide contract
<repo>/packages/foo/AGENTS.md         ← project: foo-specific additions
<repo>/packages/foo/src/AGENTS.md     ← sub-project: only when genuinely needed
```

Three layers is usually the maximum useful depth. Avoid nesting deeper unless each layer adds genuinely distinct, non-overlapping guidance.

---

## Scope rule

**For agents that accumulate layered files, each file covers only what is true at its scope and not already covered by its parent.**

| Level | Covers |
|-------|--------|
| Root | Monorepo identity, workspace tooling, cross-project conventions, repo topology, global boundaries |
| Project | Project identity, local tech stack (only what diverges from root), local workflow, local boundaries |
| Sub-project | Rarely needed; only when the sub-directory has concerns distinct from both root and project |

---

## Root responsibilities in a monorepo

Root `AGENTS.md` should cover:

- **What the monorepo is for** — mission, platform role, product family (abstract and durable).
- **Repo topology** — where projects live (`packages/`, `apps/`, `services/`, etc.) and how to navigate.
- **Workspace tooling** — package manager (`pnpm`, `turborepo`, `nx`), root-level scripts, CI entry points.
- **Cross-project conventions** — commit style, PR workflow, shared linting/formatting config.
- **Global boundaries** — danger zones and critical facts that apply repo-wide.
- **Discovery pointer** — note that each project has its own `AGENTS.md` for project-specific guidance.

Root should **not** inline:

- Any individual project's tech stack or local workflow.
- Project-specific boundaries that don't affect other projects or the repo at large.
- Operational details that differ per project.

---

## Project-level responsibilities

Project `AGENTS.md` should cover:

- **What this project is** — one sentence; assume the agent already knows the monorepo context from root.
- **Local tech stack** — only what differs from or adds to root-level conventions.
- **Local commands** — build, test, lint, dev-server commands specific to this project.
- **Local boundaries** — do-not-touch paths, irreversible operations, critical invariants for this project only.
- **Companion doc pointers** — links to project-specific architecture, schema, or deployment docs.

Project file should **not** repeat:

- Monorepo identity (already in root).
- Global boundaries (already in root).
- Root-level workflow steps that apply unchanged.

---

## No-repeat rule

Never duplicate guidance between layers. If root says "use `pnpm` workspaces", project files do not mention `pnpm` unless the project diverges.

When a project **overrides** a root convention, make the override explicit:

```markdown
> Override: this package uses Bun for local dev. Run `bun run dev` instead of the root-level `pnpm dev`.
```

---

## Multi-layer depth

For deeply nested architectures (e.g., `apps/platform/services/auth/`):

- Only add a new `AGENTS.md` layer when the concern is genuinely new and local to that scope.
- Avoid files whose only content is "see parent AGENTS.md" — that adds noise with no signal.
- Group-level files (e.g., `apps/AGENTS.md`) are useful when a group of projects shares conventions not shared repo-wide.

---

## Analysis checklist for layered setups

When analyzing a monorepo's instruction files:

1. Identify the target agent and verify file names, traversal, precedence, and merge behavior.
2. Map all instruction files and their directory depth.
3. Check root: is it acting as a project-detail aggregator instead of a monorepo contract?
4. Check projects: do they restate root content verbatim?
5. Check boundaries: global danger zones in root; project-local danger zones in project files.
6. Check topology: does root describe where projects live so agents know where to look?
7. Check overrides: are project-level divergences from root conventions explicitly flagged?

---

## Rewrite sequence for layered setups

When converting a flat `AGENTS.md` to a layered setup:

1. Identify monorepo-wide concerns and keep them in root.
2. Extract project-specific content and create `<project>/AGENTS.md` for each distinct project.
3. Deduplicate: remove from project files anything already established by root.
4. Add a discovery pointer at root: "Each project has its own `AGENTS.md`; read it when working inside that project."
5. For project files that override root conventions, mark the override explicitly.
6. Verify: an agent reading root + one project file gets sufficient context for that project without redundancy.
