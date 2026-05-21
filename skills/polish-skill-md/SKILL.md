---
name: polish-skill-md
description: 'Use when refining, polishing, reviewing, linting, or rewriting an existing SKILL.md so it triggers reliably and stays concise. Applies the five Skill design patterns (Tool Wrapper, Generator, Reviewer, Inversion, Pipeline), the agentskills.io spec, progressive disclosure, and description-triggering best practices. Do NOT use to scaffold a brand-new skill from scratch.'
---

# Polish `SKILL.md`

Refine an existing `SKILL.md` into a **tight, well-triggered, pattern-aligned** skill manifest.

This skill is itself a **Reviewer + Generator + Pipeline**:

- **Reviewer**: grade the target `SKILL.md` against an external rubric.
- **Generator**: rewrite using a fixed structural template.
- **Pipeline**: `intake → classify → review → revise → verify`, with a gate between review and revise.

---

## Use When

Use when the user asks to:

- polish, refine, tighten, lint, or review a `SKILL.md`
- improve a skill's `description` so it triggers correctly
- shorten an overgrown `SKILL.md` via progressive disclosure
- realign a skill to the five Skill design patterns
- audit an existing skill before publishing

Do **not** use to:

- author a brand-new skill from a blank page (use a scaffolding skill)
- edit ordinary application code, `AGENTS.md`, or `DESIGN.md` files

---

## Operating Modes

- **`review`** - diagnose only. Produce findings + revision plan. No edits.
- **`polish`** - apply the revision plan. Edit `SKILL.md` and split reference files.

If intent is ambiguous, default to **`review`**.

---

## The Five Skill Design Patterns

Every `SKILL.md` should consciously map to one or more of these. Drift between claimed pattern and actual body is a defect. Most production skills compose 2-3 patterns.

| Pattern | Purpose | Signal it applies | Directories |
|---|---|---|---|
| **Tool Wrapper** | Encapsulate an API / library / CLI surface | Skill is "how to use X"; value is recall | `references/` |
| **Generator** | Produce structured artifacts from a template | Skill outputs a fixed-shape document | `references/` + `assets/` |
| **Reviewer** | Grade work against an external checklist | Skill is "audit / lint / critique X" | `references/` (+ optional `scripts/`) |
| **Inversion** | Interview the user before acting | Skill needs context the user holds | `assets/` (+ optional `references/`) |
| **Pipeline** | Chain the above with gate conditions | Skill has ordered phases with checkpoints | composes others |

Directory mismatch (e.g. Generator with no `assets/`, Pipeline with no gates) is a drift signal.

Full pattern guidance, public examples, and the Gate Test: [references/patterns.md](./references/patterns.md). Source citations: [references/sources.md](./references/sources.md).

---

## Pipeline (with gate)

```
intake → classify → review ──[GATE: user confirms plan in `review` mode]──▶ revise → verify
```

1. **intake** - read the target `SKILL.md` and any `references/` siblings.
2. **classify** - identify which of the five patterns the skill claims and which it actually implements.
3. **review** - score against [references/rubric.md](./references/rubric.md). Produce a keep / tighten / split / delete plan.
4. **GATE** - in `review` mode, stop here and return the plan. In `polish` mode, proceed only if the plan is unambiguous; otherwise downgrade to `review` and ask.
5. **revise** - apply edits using the structural template in [references/workflow.md](./references/workflow.md).
6. **verify** - run the final checks listed below.

---

## Execution

Load on demand:

1. **Five patterns, examples, Gate Test:** [references/patterns.md](./references/patterns.md) - load during `classify`.
2. **Review rubric and severity order:** [references/rubric.md](./references/rubric.md) - load during `review`.
3. **Polish workflow and SKILL.md template:** [references/workflow.md](./references/workflow.md) - load during `revise`.
4. **Word-tightening rules:** [references/word-tightening.md](./references/word-tightening.md) - load during `revise`, after structural fixes, before final verification.
5. **Source citations:** [references/sources.md](./references/sources.md) - load only if defending a claim or escalating a finding.

Always run `classify` before `review`. Always run `review` before `revise`.

---

## Core Rules

- **Description carries triggering.** It is the only field the agent sees at selection time. Imperative phrasing, user-intent framing, ≤ 1024 chars. See [references/rubric.md](./references/rubric.md#description).
- **SKILL.md ≤ ~500 lines / ~5,000 tokens.** Overflow → progressive disclosure into `references/`, `assets/`, or `scripts/`.
- **Add what the agent lacks; omit what it already knows.** No PDF-is-a-format prose.
- **One default, brief alternatives.** Never a menu of equal options.
- **Procedures over declarations.** Teach a method, not a one-off answer.
- **Pattern fidelity.** If the skill claims Reviewer, it must reference an external checklist, not embed grading prose inline.
- **Pointers over copies.** Link to reference files, scripts, and external docs.
- **Tighten last, not first.** After structural fixes are settled, apply [references/word-tightening.md](./references/word-tightening.md) to cut filler, hedges, and weak vocabulary without changing imperatives, numeric thresholds, or gate conditions.
- **Preserve the skill's `name`** unless the user explicitly asks to rename.

---

## Deliverables

### `review` mode

Return:

- declared vs actual pattern(s)
- description triggering analysis (too narrow / too broad / well-scoped)
- rubric findings, ordered by severity
- keep / tighten / split / delete classification per section
- proposed new structure (SKILL.md outline + companion files)
- explicit open questions blocking `polish`

Do **not** edit files.

### `polish` mode

Deliver:

- a rewritten `SKILL.md` matching the template in [references/workflow.md](./references/workflow.md)
- new or updated files under `references/` / `assets/` / `scripts/` as required
- a short changelog: what was kept, tightened, split, deleted, and why
- the verification checklist below, ticked

---

## Verification (run before declaring done)

- [ ] YAML front matter has `name` (kebab-case) and `description` (≤ 1024 chars, imperative).
- [ ] `description` lists concrete trigger phrases and at least one negative scope ("do not use for…").
- [ ] SKILL.md is ≤ ~500 lines.
- [ ] Every `references/` file is linked from SKILL.md with an explicit *when to load* cue.
- [ ] Each major section maps to one of the five patterns, or is explicitly meta (Use When / Modes / Verification).
- [ ] No giant inline code dumps that belong in `scripts/` or `assets/`.
- [ ] No menu-of-equals; every choice has a default.
- [ ] Discovery sanity check: `npx skills add "<repo-root>" --list` shows the polished skill.

---

## Final Standard

Result: a `SKILL.md` that a cold agent can read once, classify in seconds, and execute without rereading. Pattern-aligned, trigger-tight, progressively disclosed.
