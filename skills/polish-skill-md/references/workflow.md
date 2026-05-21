# Polish Workflow

Mechanical steps for the `polish` mode pipeline. `review` mode stops after step 4.

---

## 1. Intake

- Read the target `SKILL.md` end-to-end.
- Enumerate sibling files: `references/*`, `assets/*`, `scripts/*`.
- Note current size (lines, approximate tokens) and front-matter fields.

## 2. Classify

For the skill, answer:

- Which of the five patterns does the **description** claim?
- Which patterns does the **body** actually implement?
- Where does it drift (claim ≠ body)?

Record drift - it becomes the highest-priority revision target after triggering defects.

## 3. Review (apply [rubric.md](./rubric.md))

Produce, in this order:

1. Triggering analysis on the `description`.
2. Pattern-drift findings.
3. Spec compliance (front matter, length, progressive disclosure).
4. Section-by-section keep / tighten / split / delete table.
5. Proposed new outline (SKILL.md + companion files).
6. Open questions blocking polish.

## 4. Gate

- `review` mode: **stop**. Return findings + plan.
- `polish` mode: proceed only if open questions are empty. Otherwise downgrade to `review` and ask the user.

## 5. Revise

Apply edits using the template below. Order of operations:

1. Rewrite `description` first - it gates everything else.
2. Rewrite the SKILL.md body to match the template.
3. Move split content into `references/<topic>.md` files; add load cues in SKILL.md.
4. Extract scripts > ~30 lines into `scripts/`.
5. Extract output templates into `assets/`.
6. **Tighten.** With structure now stable, apply [word-tightening.md](./word-tightening.md) in its prescribed order: restructure → filler → vocabulary → final read. Never tighten before structural fixes - you will polish prose that should have been split or deleted.
7. Update or create a short changelog at the end of your response (not in the file).

## 6. Verify

Run the SKILL.md verification checklist. Then:

```bash
npx skills add "<repo-root>" --list
```

Confirm the polished skill is discovered.

---

## SKILL.md Template

Use this skeleton. Omit sections only when the skill genuinely has nothing to put there.

```markdown
---
name: <kebab-case-name>
description: '<Imperative. Trigger phrases. Negative scope. ≤ 1024 chars.>'
---

# <Human Title>

<One-paragraph statement: what the skill does and which pattern(s) it implements.>

---

## Use When

Use when the user asks to:

- <concrete trigger 1>
- <concrete trigger 2>
- <concrete trigger 3>

Do **not** use for:

- <adjacent task 1>
- <adjacent task 2>

---

## Operating Modes  <!-- omit if single-mode -->

- **`<mode-a>`** - <what it does, what it does NOT touch>
- **`<mode-b>`** - <what it does>

If intent is ambiguous, default to **`<safest mode>`**.

---

## Pipeline  <!-- omit if not a Pipeline skill -->

`<phase1> → <phase2> ──[GATE]──▶ <phase3> → <phase4>`

Describe each gate: who decides, what unblocks it, what failure does.

---

## Execution

Load on demand:

1. **<topic>:** [references/<file>.md](./references/<file>.md)
2. **<topic>:** [references/<file>.md](./references/<file>.md)

State any always-run command:

```bash
<command>
```

---

## Core Rules

- <Load-bearing rule 1 - the agent will get it wrong without this.>
- <Load-bearing rule 2.>

---

## Deliverables

### <mode-a>

- <artifact>

### <mode-b>

- <artifact>

---

## Verification

- [ ] <check 1>
- [ ] <check 2>

---

## Final Standard

<One sentence: what "good" looks like.>
```

---

## Common Revisions

| Symptom | Action |
|---|---|
| Description is "This skill does X" | Rewrite to "Use when the user asks to X…" |
| Description has no negative scope | Add "Do not use for…" line |
| SKILL.md > 500 lines | Split largest sections to `references/`, add load cues |
| Two reference files always loaded together | Merge them |
| Reference file never linked from SKILL.md | Link with a load cue, or delete |
| Long bash block in SKILL.md | Move to `scripts/<name>.sh`, reference path |
| "Best practices" prose | Replace with concrete rules or delete |
| Menu of equal libraries / approaches | Pick a default, demote rest to one-line alternative |
| Reviewer skill grading inline | Move rubric to `references/rubric.md` |
| Generator skill with inline template > 30 lines | Move template to `assets/` |
| Hedges, weak modifiers, throat-clearing prose | Apply [word-tightening.md](./word-tightening.md) filler pass |
| "utilize" / "leverage" / "in order to" / "make use of" | Vocabulary pass per [word-tightening.md](./word-tightening.md) |
| Enumerative paragraphs or key/value prose | Convert to bullets or tables (structure pass) |
