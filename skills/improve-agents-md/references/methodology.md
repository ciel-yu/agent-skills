# Improve `CLAUDE.md` Methodology

## Core Model

Treat the root instruction file as an **onboarding contract** for every session.

Answer:

| Dimension | What it should cover |
|-----------|----------------------|
| **WHY** | What the repository is for; key subsystem purpose |
| **WHAT** | Tech stack, repository map, module boundaries |
| **DIRECTION** | Current canonical patterns, legacy-only areas, conflict resolution |
| **HOW** | Default commands, verification flow, workflow constraints |

If content does not help most tasks, it likely does **not** belong in root.

---

## Principles

### 1. Less is more

The root file enters every session. Extra instructions dilute attention.

- Keep the root file concise.
- Keep only broadly applicable guidance.
- Remove repetitive policy prose and low-value detail.

### 2. Universal over local

Prefer guidance for most tasks:

- repository identity
- codebase map
- default build/test/verify flow
- important boundaries

Avoid root content like:

- feature-specific implementation notes
- one-off debugging steps
- long style guides
- copied examples that may drift

### 3. Clarify current direction

When a repository contains multiple historical patterns, root instructions should name the current direction for new work.

This is not a full history. It is a short set of decision rules that tells the agent which patterns are canonical, which are legacy-only, and how to behave when examples conflict.

### 4. Progressive disclosure

Move narrow or deep instructions into separate Markdown docs.

Examples:

```text
agent_docs/
  running-tests.md
  code-conventions.md
  service-architecture.md
  deployment.md
  schema-rules.md
```

Root should point to those docs and say to read them **only when relevant**.

### 5. Prefer pointers to copies

Prefer:

- file paths
- `file:line` references when useful
- links to authoritative docs

Avoid large code snippets unless they are the most stable, compact rule format.

### 6. Claude is not a linter

Do not use root instructions as a substitute for:

- formatters
- linters
- typecheckers
- tests
- hooks

If tooling can enforce a rule, prefer tooling.

### 7. Make conditional guidance explicit

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

### 8. Hand-tuned beats auto-generated

You may generate a draft, but the final root file should be curated.

Every root line affects every session.

---

## Optional Target Section: Current Direction

Use `## Current Direction` when the repository contains older and newer patterns, or examples that conflict.

Keep it to short, stable decision rules:

- what to use for new code
- what is legacy-only or maintenance-only
- when to preserve local patterns versus migrate
- which docs, directories, or modules are canonical when examples conflict

Prefer concrete `if/then` rules, paths, and "do not copy X for new work" boundaries. Avoid abstract `Key Principles` lists that do not change decisions.

Example format:

```markdown
## Current Direction

This repo contains older and newer patterns. For new work, follow these decision rules:

- If adding a new API endpoint, use `src/server/routes/`; treat `legacy/api/` as maintenance-only.
- If adding UI, use function components, hooks, and shared design tokens; do not copy class-component examples.
- If modifying legacy code, preserve behavior and avoid broad migration unless the task explicitly asks for it.
- If examples conflict, prefer `docs/architecture.md` and the newest modules under `src/features/`.
```

---

## Required Classification Step

Before proposing or rewriting, classify each current section:

1. **Keep in root**
2. **Move to companion doc**
3. **Convert to conditional block**
4. **Delete as noise**

Direction-related content belongs in root only when it helps choose between competing patterns across many tasks; move history and rationale to companion docs.

Do not skip this step.
