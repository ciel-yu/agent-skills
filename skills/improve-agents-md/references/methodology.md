# Improve `CLAUDE.md` Methodology

## Core Model

Treat the root instruction file as a **cross-session operating contract** for every session.

Assume the root `AGENTS.md` lives at `<repo>/AGENTS.md` and is read by many agents across many sessions. Write for persistence.

In a monorepo or multi-project repository, `AGENTS.md` files exist at multiple depths. Each layer adds guidance scoped to its directory; parent layers are always read first. See [layered-setup.md](./layered-setup.md) for the full hierarchy model.

Answer:

| Dimension | What it should cover |
|-----------|----------------------|
| **WHY** | What the repository is for; abstract goal, mission, or durable objectives |
| **WHAT** | Tech stack, repository map, module boundaries |
| **DIRECTION** | Which pattern is canonical for new work, which is authoritative, how to resolve conflicts |
| **HOW** | Default commands, verification flow, workflow constraints |
| **BOUNDARIES** | Hard constraints; dangerous zones; irreversible operations; critical facts that prevent silent mistakes |

If content does not help most tasks, it likely does **not** belong in root.

If content is likely to change with the next work session, it almost certainly does **not** belong in root.

In a monorepo, if content applies to only one project, it belongs in that **project's** `AGENTS.md`, not in root.

---

## Principles

### 1. Less is more

The root file enters every session. Extra instructions dilute attention.

- Keep the root file concise.
- Keep only broadly applicable guidance.
- Remove repetitive policy prose and low-value detail.

**Exception:** dangerous zones and critical facts that belong in `## Boundaries` are exempt from length pressure. A short, concrete constraint that prevents irreversible harm earns its place in root regardless of how specific it appears.

### 2. Universal over local

Root content should serve most tasks: repository identity, codebase map, default workflow, key boundaries. Move feature notes, one-off debugging steps, style guides, and drifting examples to companion docs.

This includes **working state**: active task lists, current progress notes, in-flight implementation details, and narrow factual snapshots. Root may point to where those live; root should not become their storage layer.

### 3. Clarify current direction

When a repository contains multiple competing conventions, tools, or patterns, root instructions should name the current direction for new work.

This is not a full history. It is a short set of decision rules that tells the agent which patterns are canonical, which are authoritative, and how to behave when examples conflict.

`## Current Direction` is for **persistent decisions**, not transient state. Good contents: canonical stack choices, legacy-vs-new boundaries, source-of-truth rules, migration posture, preservation rules. Bad contents: "currently implementing X", "latest progress", "remaining tasks", or other time-sensitive updates.

Heuristic: if the line would need updating after a normal day of work, it is probably status, not direction.

### 4. Goals should be abstract, not session-bound

When describing the repository's overall goal, prefer durable abstractions:

- mission
- product objective
- system role
- operating constraints that remain true across sessions

Avoid session-bound material:

- current progress
- implementation details
- temporary facts that will drift
- narrow milestone tracking

Good:

```markdown
## Purpose

This repo maintains the shared agent operating model for `<repo>`, with reusable policies and references that should remain valid across sessions.
```

Bad:

```markdown
## Purpose

We are currently migrating the prompt format and have finished the root rewrite but still need to update three nested files.
```

### 5. Progressive disclosure

Move narrow or deep instructions into separate Markdown docs. Root should point to them and say to load them **only when relevant**.

This applies especially to changing state. Root should say **where to find** active plans, status docs, issue trackers, or handoff notes rather than copying that material inline.

### 6. Prefer pointers to copies

Prefer file paths, `file:line` references, and links to authoritative docs over large copied snippets.

### 7. Prefer tooling

If a rule can be enforced by a formatter, linter, typechecker, hook, or test, prefer tooling over root instructions.

### 8. Make conditional guidance explicit

Some instructions apply only in narrow cases. Wrap them in conditional blocks, not vague prose.

Example:

```html
<important if="you are writing or modifying tests">
- Use the shared integration test helpers.
- Prefer existing fixtures before creating new ones.
</important>
```

Guidelines:

- Do not wrap foundational repo identity or structure.
- Conditions must be specific.
- Avoid useless conditions like `if="you are writing code"`.

### 9. Hand-tuned beats auto-generated

You may generate a draft, but the final root file should be curated.

Every root line affects every session.

### 10. Prefer mechanism and strategy over state snapshots

When improving `AGENTS.md`, bias toward durable operating mechanisms:

- how to locate current work state
- how to resolve conflicting examples
- how to choose canonical patterns
- how to escalate risky operations
- how to discover deeper task-specific guidance

Prefer these over state snapshots like:

- what is being worked on this week
- which implementation step is half-done
- exact temporary facts that will be stale soon

Good:

```markdown
## Additional Context

- For active work status, read `docs/status/current.md` and the linked issue tracker before editing.
- For architecture decisions, prefer `docs/architecture.md` over older examples in `archive/`.
```

Bad:

```markdown
## Current Direction

- We are halfway through the auth refactor.
- Next step is to update the session cache.
```

### 11. Thinking discipline is not a section

Concrete, rule-shaped "thinking discipline" (e.g. *reproduce with a failing test first*, *do not copy from `legacy/` for new work*, *ask before irreversible changes*) is welcome — but it belongs **inside existing sections**, not under a new `## Thinking Discipline` / `## Reasoning Framework` heading.

Before admitting a discipline rule into root, it must pass three filters:

1. **Universal** — changes behavior on *most* tasks (otherwise → conditional block or companion doc).
2. **Not tool-replaceable** — cannot be enforced by a linter, formatter, hook, typechecker, or test (otherwise → tooling).
3. **Not already model-default** — reverses a plausible default behavior (otherwise → noise; the model already does it).

Placement rules:

| Discipline shape | Goes into |
|---|---|
| Ordered steps (reproduce → minimal change → verify) | `## Default Workflow` |
| Hard "do not cross" lines | `## Boundaries` |
| Canonical-vs-legacy choices | `## Current Direction` |
| Task-scoped rules | `<important if="...">` conditional block |

Heuristic: *if a rule needs the heading `## Thinking Discipline` to justify its existence, it is not yet concrete enough.* Truly concrete discipline self-sorts into the sections above.

---

## Optional Target Section: Current Direction

Use `## Current Direction` when the repository contains **competing conventions, tools, workflows, or patterns** that an agent must choose between — regardless of whether the conflict is architectural, methodological, or toolchain-level.

Keep it to concrete `if/then` rules: what is canonical for new work, what is authoritative when sources conflict, what is read-only or legacy-only, when to preserve existing patterns.

Treat this section as a **decision register**, not a live progress report.

Avoid abstract `Key Principles` lists that do not change decisions.
Avoid current status, milestone tracking, implementation progress, and temporary facts that will require frequent refresh.

**Examples** (software development / reverse engineering / data mining):

```markdown
## Current Direction

This repo contains older and newer patterns. For new work, follow these decision rules:

- If adding a new API endpoint, use `src/server/routes/`; treat `legacy/api/` as maintenance-only.
- If adding UI, use function components, hooks, and shared design tokens; do not copy class-component examples.
- If modifying legacy code, preserve behavior and avoid broad migration unless the task explicitly asks for it.
- If examples conflict, prefer `docs/architecture.md` and the newest modules under `src/features/`.
```

```markdown
## Current Direction

This repo targets multiple binaries with different toolchains. Follow these decision rules:

- If analyzing a new binary, start with static analysis in Ghidra; use dynamic tracing only when static analysis hits a wall.
- Document findings in `findings/<target>/notes.md`; do not commit raw IDB files to main branch.
- If toolchain examples conflict, prefer scripts under `tools/` over ad-hoc one-liners.
- Treat `archive/` as read-only reference material; do not copy patterns from it for active targets.
```

```markdown
## Current Direction

This repo contains both exploratory notebooks and production pipelines. Follow these decision rules:

- For new analysis, use scripts under `pipelines/`; treat notebooks in `notebooks/explore/` as scratch space, not canonical.
- Raw data lives in `data/raw/` and is immutable; always write outputs to `data/processed/`.
- If a data source has both a v1 and v2 collector, use v2; v1 is maintenance-only.
- When findings need to be shared, output to `reports/` as Markdown; do not rely on notebook output cells as deliverables.
```

---

## Optional Target Section: Boundaries

Use `## Boundaries` for two categories of content that must stay in root regardless of length pressure.

**Dangerous zones** — actions where a mistake is irreversible or has outsized consequences: do-not-modify paths, files, systems, or branches; operations requiring explicit confirmation; environments or data that must not be touched autonomously.

**Critical facts** — non-obvious invariants needed to avoid silent mistakes: shared state between systems, hard external contracts, mandatory prerequisites that cannot be inferred from code.

**Placement:** if violating a rule causes irreversible damage, data loss, or unauthorized access, it belongs in root `## Boundaries` regardless of how narrow it seems. If a constraint only matters for one specific task, move it to a companion doc. Do not use Boundaries for style preferences, single-subsystem warnings, or aspirational constraints with no observable meaning (`"be careful with X"`).

**Examples** (software development / reverse engineering / data mining):

```markdown
## Boundaries

- Do not push directly to `main` or `release/*`; all changes go through pull requests.
- Do not modify `infra/terraform/` without explicit task scope; changes are applied automatically on merge.
- `config/secrets.example` is a template; never commit real credentials.
- The public REST API under `src/api/v1/` is consumed by external clients; do not change its contract without a versioning plan.
```

```markdown
## Boundaries

- Do not execute untrusted samples on the host machine; use the isolated VM at `vms/analysis-vm`.
- Do not modify files under `targets/` directly; work only in `workspace/<target>/`.
- `findings/` is append-only; do not rewrite or delete existing entries.
- Do not upload samples or findings to external services without explicit instruction.
```

```markdown
## Boundaries

- `data/raw/` is immutable; never write, modify, or delete files in it.
- Do not run collection scripts against live sources without rate-limit configuration set.
- Do not commit API keys, credentials, or PII to the repository.
- `reports/` is the only output directory cleared for external sharing; do not share from `data/processed/` directly.
```

---

## Required Classification Step

Before proposing or rewriting, classify each current section:

1. **Keep in root**
2. **Move to companion doc**
3. **Convert to conditional block**
4. **Delete as noise**

Direction-related content belongs in root only when it helps choose between competing patterns across many tasks; move history and rationale to companion docs.
State-related content belongs in root only when it describes a durable operating mechanism for finding state; otherwise move it to a companion doc, tracker, or handoff artifact.

Do not skip this step.
