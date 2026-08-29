---
name: polish-skill-md
description: 'Use when reviewing, linting, tightening, or rewriting an existing SKILL.md and its bundled resources. Check Agent Skills specification compliance, triggering, progressive disclosure, script safety, and evidence from real evaluations; apply edits only when requested. Do not use for brand-new skill design, AGENTS.md, general Markdown, or application code.'
---

# Polish `SKILL.md`

Review or polish an existing Agent Skill against the specification, target-client behavior, and observed task results. Do not force a taxonomy or claim improvement without evaluation.

## Use When

Use when the user asks to:

- review, lint, tighten, or rewrite an existing `SKILL.md`
- improve description triggering or reduce false activation
- split detailed content into on-demand resources
- audit bundled scripts, references, assets, and links before publishing
- compare an edited skill with its prior version or a no-skill baseline

Do **not** use for:

- designing a brand-new skill from a blank page
- `AGENTS.md` or other repository instruction files
- general Markdown cleanup or application code

## Modes and Depth

- **`review`** — report findings and a revision plan; edit nothing
- **`polish`** — apply an evidence-backed revision and validate it

Choose the smallest sufficient path:

- **Focused:** a named frontmatter, wording, link, or resource issue. Inspect `SKILL.md` and the affected dependency chain.
- **Full:** broad review, publishing audit, restructuring, or behavior change. Inspect every bundled file.

If edits are not clearly authorized, use `review`.

## Pipeline

`intake → choose focused/full → inspect → grade → review: report | polish: edit → validate → evaluate`

- **Authority gate:** Resolve conflicts in this order: user and repository requirements → Agent Skills specification and target-client docs → observed evaluations → official authoring guidance → optional design lenses.
- **Mode gate:** `review` stops after findings and plan. `polish` edits without another approval when scope is clear.
- **Evidence gate:** Do not call a description better-triggered or a skill more effective unless the relevant evaluation ran. Otherwise label the result static-only.

## Execution

Load only what the task needs:

1. **Specification, severity, content, and security:** [references/rubric.md](./references/rubric.md) — relevant sections for focused work; entire file for full review
2. **Trigger and output evaluation:** [references/evaluation.md](./references/evaluation.md) — when description, workflow, outputs, or performance claims change
3. **Optional structural vocabulary:** [references/design-lenses.md](./references/design-lenses.md) — only when a design lens helps explain a structural choice; never for compliance or deletion decisions
4. **Source index:** [references/sources.md](./references/sources.md) — when verifying a standard, client behavior, or external claim

## Core Rules

- **Preserve identity.** Keep `name` and directory identity unless the user explicitly requests a rename; migrate every internal reference if renamed.
- **Fix consequential defects first.** Safety, specification, triggering, broken execution, and stale resources outrank style.
- **Do not impose a body template.** The specification permits any body content that helps the agent perform the task.
- **Write descriptions for selection.** State what the skill does and when to use it. Add negative scope only when adjacent requests create real false positives.
- **Spend context on missing expertise.** Prefer procedures, calibrated defaults, invariants, quirks, and validation over generic explanations.
- **Use progressive disclosure.** Keep always-needed instructions in `SKILL.md`; move conditional depth to directly linked resources with explicit load cues.
- **Match control to fragility.** Give flexible tasks room; give brittle operations exact commands, checks, and recovery paths.
- **Provide a fast path.** A heavy pipeline needs applicability boundaries, optional steps, and a simpler fallback for routine cases.
- **Treat the bundle as software.** Inspect scripts, external URLs, dependencies, permissions, and data flows; never run suspicious setup during review.
- **Prefer clean cutovers.** Remove obsolete text, files, links, and aliases after migrating every use.

## Deliverables

### `review`

- findings ordered by severity with exact evidence
- specification and target-client status
- triggering analysis and evaluation status
- keep / tighten / split / delete plan
- security and operability findings across inspected resources
- explicit **No findings** when warranted

### `polish`

- updated `SKILL.md` and only necessary bundled resources
- record of kept, changed, moved, and removed content with reasons
- validation and evaluation results, distinguishing observed behavior from static inference

## Verification

- [ ] `name`, directory name, `description`, and optional frontmatter satisfy the current specification and target client.
- [ ] Description states both capability and usage context without keyword stuffing.
- [ ] Every instruction contributes expertise, control, or verification the agent otherwise lacks.
- [ ] Conditional resources are directly linked with when-to-load cues; no broken or deep reference chains remain.
- [ ] Scripts are non-interactive, dependency-aware, testable, and documented through useful inputs, outputs, and errors.
- [ ] External content, network calls, permissions, credentials, and destructive operations were audited.
- [ ] Heavy workflows expose applicability boundaries and a lightweight fallback.
- [ ] Relevant trigger or output evaluations ran, or the untested behavior is explicitly identified.
- [ ] The official validator passes; repository discovery is confirmed when the repository publishes through an installer.
- [ ] `review` changed nothing; `polish` changed only the agreed skill scope.

## Final Standard

The result is a valid, secure, focused skill whose claimed improvements match observed evidence.
