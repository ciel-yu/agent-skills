---
name: improve-agents-md
description: 'Use when creating, reviewing, minimizing, rewriting, or splitting AGENTS.md repository instructions. Ground every rule in repository or user evidence, verify the target client loading semantics, keep only non-inferable guidance that changes behavior, and scope root versus nested files. Do not use for CLAUDE.md, GEMINI.md, general documentation, application code, or SKILL.md.'
---

# Improve `AGENTS.md`

Audit or rewrite `AGENTS.md` as a minimal, versioned set of repository-specific instructions. A no-change result—or recommending no file—is valid. Do not generate a generic repository handbook.

## Use When

Use when the user asks to:

- create, review, reduce, rewrite, or split `AGENTS.md`
- remove stale, duplicated, inferred, or task-specific instructions
- choose root versus nested `AGENTS.md` scope
- diagnose conflicts or loading-order problems
- preserve critical repository constraints while cutting context cost

Do **not** use for:

- `CLAUDE.md`, `GEMINI.md`, or product-specific instruction formats
- application code or general project documentation
- skill manifests (`SKILL.md`)

## Modes

- **`analyze`** — report findings and a target layout; edit nothing
- **`rewrite`** — create or edit only the authorized `AGENTS.md` files

If the request does not authorize edits, use `analyze`.

## Pipeline

`identify client → inspect evidence → apply value gate → choose scope → analyze: report | rewrite: edit → verify`

- **Semantics gate:** Do not design layers until the target client traversal, precedence, merge behavior, and size limits are confirmed.
- **Evidence gate:** Every retained or added instruction needs a repository or user source and must pass the value gate in [references/method.md](./references/method.md).
- **Mode gate:** `analyze` stops after the report. `rewrite` proceeds without another approval when the user already authorized the rewrite and no material choice remains.

## Execution

Load only when needed:

1. **Evidence, value, scope, and enforcement:** [references/method.md](./references/method.md) — always for substantive review or rewrite
2. **Client discovery and precedence:** [references/client-semantics.md](./references/client-semantics.md) — when identifying or verifying the target AGENTS.md consumer
3. **Severity and output contract:** [references/rubric.md](./references/rubric.md) — during review and final verification
4. **Behavioral evaluation:** [references/evaluation.md](./references/evaluation.md) — for new setups, major rewrites, disputed rules, or explicit evaluation requests

## Core Rules

- **Prefer non-inferable facts.** Exact prerequisites, costly-command warnings, generated or immutable paths, external contracts, and canonical-versus-legacy decisions outrank summaries.
- **Require behavioral value.** Keep a line only when it prevents a plausible error, avoids measurable waste, or encodes an explicit policy at that scope.
- **Use actual loader semantics.** `AGENTS.md` is a file format, not a universal traversal algorithm. Never assume nested files accumulate or override identically across clients.
- **Keep commands selective.** Include the canonical safe command, required setup, and important cost or scope warnings—not every available command.
- **Scope instead of duplicate.** Root holds repository-wide rules. Nested files add only local differences and only when the target client loads them for the intended workflow.
- **Separate guidance from enforcement.** Markdown can guide an agent; it cannot guarantee a prohibition. Route hard controls to permissions, hooks, CI, branch protection, or sandboxing.
- **Preserve supported safety constraints.** Never weaken an explicit user or repository boundary merely to shorten the file.
- **Point to maintained sources.** Link optional detail instead of copying it, but do not imply that a link is automatically loaded or enforced.

## Deliverables

### `analyze`

- target-client loading map and any unresolved uncertainty
- findings ordered by severity with evidence
- keep / tighten / relocate / delete classification
- recommended root and nested layout, including a no-file or no-change result when warranted
- no file edits

### `rewrite`

- updated in-scope `AGENTS.md` files
- concise record of retained, removed, relocated, and added guidance with sources
- verification results and any behavior not directly tested

## Verification

- [ ] Target-client loading semantics and active instruction sources are confirmed.
- [ ] Every command, path, policy, and architecture claim has current evidence.
- [ ] Every instruction passes the value and scope gates.
- [ ] Root omits generic overview material the client can cheaply infer.
- [ ] Nested files are used only where the client actually loads them.
- [ ] Canonical commands retain prerequisites and expensive-operation warnings.
- [ ] Safety guidance is scoped correctly; required enforcement is implemented or explicitly identified outside Markdown.
- [ ] Parent and child files neither duplicate nor silently conflict.
- [ ] `analyze` changed nothing; `rewrite` touched only authorized instruction files.

## Final Standard

The result contains the smallest evidence-backed instruction set that changes agent behavior for the better.
