---
name: design-md
description: 'Use when the user asks to create, extract, apply, implement, lint, validate, review, or diff a DESIGN.md design contract. Handles YAML design tokens, markdown usage rules, current-state extraction from UI code, token mapping into CSS/Tailwind/theme systems, and contrast or regression checks. Do not use for DESIGN.md-independent UI edits, freeform visual redesigns, code review unrelated to design tokens, or skill-manifest polishing.'
---

# DESIGN.md Lifecycle

Create, extract, apply, and review `DESIGN.md`: an agent-readable design contract with YAML tokens and markdown usage rules. This skill is a multi-mode Generator + Tool Wrapper + Reviewer with a gated mode-selection pipeline.

---

## Use When

Use when the user asks to:

- create or update a `DESIGN.md`
- turn product constraints, screenshots, notes, or brand rules into design tokens
- extract current UI tokens from an existing app or component library
- map `DESIGN.md` tokens into CSS variables, Tailwind theme values, or framework theme resources
- build or restyle UI from `DESIGN.md`
- validate, lint, review, compare, or diff `DESIGN.md`

Do **not** use for:

- UI or CSS changes that do not reference `DESIGN.md`
- brand exploration or redesign when the user asks only for current-state extraction
- generic accessibility, visual design, or frontend code review without a DESIGN.md contract
- creating, polishing, or reviewing `SKILL.md` files

---

## Operating Modes

| Mode | Use when | Output |
|---|---|---|
| `create` | User wants a new design contract from constraints, screenshots, notes, or brand rules. | Authored `DESIGN.md` with exact tokens and rationale. |
| `extract` | Existing UI/code is the source of truth. | Current-state `DESIGN.md` with evidence and inferred rationale labeled. |
| `apply` | User wants UI/code changed to follow an existing `DESIGN.md`. | Styling-system mapping plus UI/code edits. |
| `review` | User wants validation, linting, comparison, or regression analysis. | Severity-ordered findings or an explicit no-findings result. |

If the request spans modes, run them in dependency order: `create` or `extract` → `review` → `apply`. If the next mode is not uniquely determined from the request and repository evidence, ask one mode-selection question before editing.

---

## Pipeline

`intake → select mode ──[MODE GATE]──▶ load mode references → execute → verify → deliver`

**MODE GATE:** The agent must identify the next mode from explicit user intent or repository evidence. If no single next mode is justified, stop and ask. Do not create or edit `DESIGN.md` or UI files before the gate passes.

---

## Execution

Load on demand:

1. **Create:** Load [authoring-workflow.md](./references/authoring-workflow.md) and [token-schema.md](./references/token-schema.md) when authoring a new `DESIGN.md` from constraints, screenshots, notes, or brand rules.
2. **Extract:** Load [extraction-workflow.md](./references/extraction-workflow.md), [source-map.md](./references/source-map.md), and [normalization-rules.md](./references/normalization-rules.md) when deriving tokens from an existing app, theme, design system, or rendered UI.
3. **Apply:** Load [implementation-workflow.md](./references/implementation-workflow.md) and [mapping-patterns.md](./references/mapping-patterns.md) when mapping tokens into CSS variables, Tailwind, component variants, or framework theme resources.
4. **Review:** Load [review-workflow.md](./references/review-workflow.md) and [quality-rubric.md](./references/quality-rubric.md) when linting, validating, comparing, or auditing `DESIGN.md`.

Use these commands when the `@google/design.md` CLI is available:

```bash
npx @google/design.md lint DESIGN.md
npx @google/design.md diff before.md after.md
npx @google/design.md export --format css-tailwind DESIGN.md
```

If the CLI is unavailable or the file path differs, run the equivalent manual checks and report the fallback.

---

## Core Rules

- **Token exactness.** Preserve exact colors, spacing, radii, font sizes, and token references. Do not approximate.
- **Existing styling system.** Map into the project's current CSS, Tailwind, component, or framework theme surface. Do not add a parallel system.
- **No extraction redesign.** In `extract`, document current state only. Mark inferred rationale as inferred.
- **DESIGN.md as contract.** Treat the YAML tokens plus markdown usage rules as the single source of truth for design implementation.
- **Operational prose.** Write rules that tell implementers where tokens apply and what to avoid; avoid generic phrases such as "modern" or "clean" unless grounded in exact choices.
- **Canonical body order.** Prefer: Overview, Colors, Typography, Layout, Elevation & Depth, Shapes, Components, Do's and Don'ts.
- **Contrast first.** Do not introduce token pairs that fail contrast when valid alternatives exist.

---

## Deliverables

### `create`

- `DESIGN.md` with valid YAML front matter and markdown usage sections
- token rationale and assumptions that affected values
- validation command results or manual-check fallback

### `extract`

- current-state `DESIGN.md`
- source summary with file paths or rendered surfaces used as evidence
- inferred rationale clearly labeled as inferred
- unresolved questions only when they change token choices

### `apply`

- changed files mapped back to `DESIGN.md` tokens
- notes for any token that could not map cleanly into the existing styling system
- UI verification summary, including responsive or interactive states touched

### `review`

- findings ordered by severity with file references
- commands run and high-level results
- suggested fixes, or an explicit no-findings result

---

## Verification

- [ ] The active mode is named and justified by user intent or repository evidence.
- [ ] Required mode references were loaded before execution.
- [ ] `DESIGN.md` has valid YAML front matter and markdown sections.
- [ ] Token references resolve and component tokens use references instead of duplicated raw values.
- [ ] Colors, typography, spacing, radii, and component tokens are exact, not approximate.
- [ ] Contrast-sensitive foreground/background pairs were checked.
- [ ] CLI validation/export/diff was run when available, or the fallback is reported.
- [ ] `apply` changes were verified on the real UI surface when possible.

---

## Final Standard

A finished `DESIGN.md` gives agents and implementers exact reusable tokens, clear usage rules, and evidence-backed rationale without inventing design intent.
