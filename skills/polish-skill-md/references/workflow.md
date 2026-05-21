# Polish Workflow

Mechanical steps for `polish` mode. `review` mode stops after step 4.

---

## 1. Intake

- Read the target `SKILL.md` end-to-end.
- List sibling files: `references/*`, `assets/*`, `scripts/*`.
- Record current size (lines, approximate tokens) and front-matter fields.

## 2. Classify

For the skill, answer:

- Which of the five patterns does the **description** claim?
- Which patterns does the **body** implement?
- Where does it drift (claim ≠ body)?

Record drift. After triggering defects, drift is the highest-priority revision target.

## 3. Review (apply [rubric.md](./rubric.md))

Produce, in this order:

1. Triggering analysis for the `description`.
2. Pattern-drift findings.
3. Spec compliance (front matter, length, progressive disclosure).
4. Section-by-section keep / tighten / split / delete table.
5. Proposed new outline (`SKILL.md` + companion files).
6. Open questions blocking polish.

## 4. Gate

- `review` mode: **stop**. Return findings + plan.
- `polish` mode: proceed only if open questions are empty. Otherwise downgrade to `review` and ask the user.

## 5. Revise

Apply edits in this order:

1. Rewrite `description` first. It gates everything else.
2. Rewrite the `SKILL.md` body to match the template.
3. Move split content into `references/<topic>.md`; add load cues in `SKILL.md`.
4. Extract scripts over ~30 lines into `scripts/`.
5. Extract output templates into `assets/`.
6. **Tighten.** Once structure is stable, apply [word-tightening.md](./word-tightening.md) in order: structure → filler → vocabulary → final read. Never tighten before structural fixes; you will polish prose that should have been split or deleted.
7. Update or create a short changelog at the end of your response, not in the file.

## 6. Verify

Run the `SKILL.md` verification checklist. Then run:

```bash
npx skills add "<repo-root>" --list
```

Confirm the polished skill is discovered.

---

## SKILL.md Template

Use this skeleton. Omit sections only when the skill truly has nothing to put there.

```markdown
---
name: <kebab-case-name>
description: '<Imperative. Trigger phrases. Negative scope. ≤ 1024 chars.>'
---

# <Human Title>

<One paragraph: what the skill does and which pattern(s) it implements.>

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

- **`<mode-a>`** - <what it does and what it does NOT touch>
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

<One sentence: what good looks like.>
```

---

## Common Revisions

| Symptom | Action |
|---|---|
| Description is "This skill does X" | Rewrite to "Use when the user asks to X..." |
| Description has no negative scope | Add a "Do not use for..." line |
| `SKILL.md` > 500 lines | Split the largest sections to `references/`, then add load cues |
| Two reference files are always loaded together | Merge them |
| A reference file is never linked from `SKILL.md` | Link it with a load cue, or delete it |
| Long bash block in `SKILL.md` | Move it to `scripts/<name>.sh`, then reference the path |
| "Best practices" prose | Replace it with concrete rules or delete it |
| Menu of equal libraries / approaches | Pick a default; demote the rest to one-line alternatives |
| Reviewer skill grading inline | Move the rubric to `references/rubric.md` |
| Generator skill with inline template > 30 lines | Move the template to `assets/` |
| Hedges, weak modifiers, throat-clearing prose | Apply the filler pass in [word-tightening.md](./word-tightening.md) |
| "utilize" / "leverage" / "in order to" / "make use of" | Apply the vocabulary pass in [word-tightening.md](./word-tightening.md) |
| Enumerative paragraphs or key/value prose | Convert them to bullets or tables (structure pass) |
