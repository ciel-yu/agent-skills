# Improve `CLAUDE.md` Methodology

## Core Model

Treat the root instruction file as an **onboarding contract** for every session.

Answer:

| Dimension | What it should cover |
|-----------|----------------------|
| **WHY** | What the repository is for; key subsystem purpose |
| **WHAT** | Tech stack, repository map, module boundaries |
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

### 3. Progressive disclosure

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

### 4. Prefer pointers to copies

Prefer:

- file paths
- `file:line` references when useful
- links to authoritative docs

Avoid large code snippets unless they are the most stable, compact rule format.

### 5. Claude is not a linter

Do not use root instructions as a substitute for:

- formatters
- linters
- typecheckers
- tests
- hooks

If tooling can enforce a rule, prefer tooling.

### 6. Make conditional guidance explicit

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

### 7. Hand-tuned beats auto-generated

You may generate a draft, but the final root file should be curated.

Every root line affects every session.

---

## Required Classification Step

Before proposing or rewriting, classify each current section:

1. **Keep in root**
2. **Move to companion doc**
3. **Convert to conditional block**
4. **Delete as noise**

Do not skip this step.
