---
name: skill-surgery
description: "Use when merging multiple skills into one, or splitting one skill into several. Handles structural analysis, companion-file relocation, description rewriting, cross-reference repair, and reverse-trigger verification. Triggers: user says 'merge these skills', 'split this skill', 'combine skills', 'break apart this skill', or asks to consolidate or decompose skill directories. Do not use for polishing a single skill (use polish-skill-md), creating a new skill from scratch (use skill-creator), or ordinary code edits."
---

# Skill Surgery

Merge or split skill directories while preserving structure, references, assets, and triggering accuracy. This skill is a **Pipeline + Reviewer**: score compatibility or detect split boundaries, propose a restructuring plan, gate on user approval, execute, verify.

---

## Use When

Use when the user asks to:

- merge, combine, or consolidate two or more skills into one
- split, decompose, or break apart a skill into several focused skills

Do **not** use for:

- polishing a single skill — use `polish-skill-md`
- creating a new skill from scratch — use `skill-creator`
- ordinary code or documentation edits

---

## Operating Modes

- **`merge`** — combine two or more skills into a single skill.
- **`split`** — decompose one skill into two or more focused skills.

If intent is ambiguous, ask before proceeding.

---

## Pipeline

```
intake → analyze → plan ──[GATE: user confirms]──▶ execute → verify
```

Each gate: **user** decides; unblocked by explicit approval; failure = loop back to plan.

---

## Execution

Load on demand:

1. **Merge strategies and description conflict detection:** [references/merge-strategies.md](./references/merge-strategies.md) — load during `analyze` and `plan` for merge operations.
2. **Split heuristics and reverse trigger testing:** [references/split-heuristics.md](./references/split-heuristics.md) — load during `analyze` and `verify` for split operations.
3. **Rollback manifest schema:** [references/migration-manifest.md](./references/migration-manifest.md) — load during `execute` when writing the manifest.

---

## Step 1: Intake

For each skill directory involved:

- Read `SKILL.md` completely.
- List companion files with their roles.
- Build a cross-reference map: for each companion file, which `SKILL.md` sections and other companion files link to it.

Output a summary table:

| Skill | Description chars | Companion files | Pattern | Links to other skills |
|---|---|---|---|---|
| ... | ... | ... | ... | ... |

---

## Step 2: Analyze

### For `merge`

Load [references/merge-strategies.md](./references/merge-strategies.md).

Score each dimension 0–10:

| Dimension | Measures | 10 | 5 | 0 |
|---|---|---|---|---|
| Domain overlap | Same domain? | Same domain, same persona | Adjacent sub-problems | Unrelated domains |
| Structural sharing | Shared companion files? | >50% shared | 20–50% | No overlap |
| Triggering clarity | Single description feasible? | Trivially combinable | Needs careful wording | Fundamentally conflicting |

**Decision rule**:
- Average ≥ 7 → proceed.
- Average 4–6 → proceed with caveats; flag risks in plan.
- Average < 4 → recommend against merging; explain why.

Build the **cross-reference heat map** — for each shared companion file, count inbound links from each skill. Files with high inbound links from multiple skills are merge-critical; handle with the Merge strategy per [references/merge-strategies.md](./references/merge-strategies.md#3-merge).

### For `split`

Load [references/split-heuristics.md](./references/split-heuristics.md).

Detect boundaries on three signals:

1. **Independent concerns** — headings cluster into groups that don't cross-reference.
2. **Size pressure** — `SKILL.md` over ~400 lines or >6 companion files.
3. **Triggering conflict** — description joins unrelated contexts with "or" >2 times, or "Do not use for" exceeds positive triggers.

Validate each candidate per [references/split-heuristics.md](./references/split-heuristics.md#validating-split-boundaries):

- **Cohesion**: own trigger context, own pipeline, ≤1024-char description.
- **Coupling**: count shared companion files. 0 = clean, 1–2 = copy, 3+ = reconsider boundary.
- **Discrimination**: no two sub-skills share the same primary trigger phrase.

Flag each boundary: **clear / possible / speculative**.

---

## Step 3: Plan

Present a concrete plan. Format varies by mode.

### Merge plan

```
Score: <average>/10 (domain: N, structural: N, triggering: N)  Risk: <low/medium/high>

Cross-reference heat map:
  <file>  ← <N> links from skill-a, <M> from skill-b  [merge-critical: yes/no]

Result: <new-skill-name>/
├── SKILL.md
├── references/
│   ├── <kept-from-A>.md          [strategy: keep / keep-stronger / merge / demote / rename]
│   └── <shared-consolidated>.md  [strategy: merge]
└── ...

Source mapping:
  <skill-a>/references/X.md → <new>/references/X.md
  <skill-a>/references/Z.md + <skill-b>/references/Z.md → [strategy] → <new>/references/Z.md

Description draft (≤ 1024 chars): "<...>"

Conflict check: ⚠ / ✓  <overlap summary if any>

Sections: keep / merge / move to references / delete — list each.
```

### Split plan

```
Boundaries:
  Boundary 1: <description> [clear/possible/speculative]
    Cohesion: ✓  Coupling: <N> shared  Discrimination: ✓

Result:
  <skill-a>/ → SKILL.md + references/...
  <skill-b>/ → SKILL.md + references/...

Source mapping:
  § X → <skill-a>  ·  § Y → <skill-b>
  <references/Z.md> → <skill-a>  ·  <references/V.md> → both (copy)

Description drafts:
  <skill-a>: "<...>"  (≤ 1024 chars)
  <skill-b>: "<...>"

Reverse trigger test plan:
  "<prompt 1>" → expected: <skill-a>
  "<prompt 2>" → expected: <skill-b>
  "<prompt 3>" → expected: <skill-...>
```

---

## Step 4: Gate

Present: analysis results, heat map, conflict check, full plan, draft descriptions, trigger test plan.

**Do not create, move, or edit any files until the user explicitly approves.**

If the user requests changes, update the plan and re-present. Loop until approved.

---

## Step 5: Execute

After approval:

1. **Create** target directories.
2. **Copy** companion files per plan. Apply the merge strategy from [references/merge-strategies.md](./references/merge-strategies.md) for duplicate references.
3. **Write** new `SKILL.md` files: kebab-case `name`, approved `description` (≤ 1024 chars), restructured body with corrected internal links.
4. **Fix cross-references**: every `[link](./references/X.md)` must resolve within the same skill directory.
5. **Write** `migration-manifest.json` per [references/migration-manifest.md](./references/migration-manifest.md).
6. **Do not delete** original skill directories.

---

## Step 6: Verify

### Structural

For every resulting skill:

- [ ] Front matter: `name` (kebab-case) + `description` (imperative, ≤ 1024 chars, ≥ 1 negative scope).
- [ ] `SKILL.md` under ~500 lines.
- [ ] Every linked companion file exists in the same directory.
- [ ] No broken internal links.
- [ ] No orphan companion files.

### Description conflict detection

Run the procedure from [references/merge-strategies.md](./references/merge-strategies.md#description-conflict-detection):

1. Read every other skill's `description` in the repo.
2. Extract positive trigger phrases from each.
3. Flag pairs sharing ≥ 3 identical triggers, or positive-vs-negative collisions.
4. If conflicts exist, refine and re-verify.

### Reverse trigger testing

Run the procedure from [references/split-heuristics.md](./references/split-heuristics.md#reverse-trigger-testing):

1. For each test prompt from the plan, determine which resulting skill should handle it.
2. Verify the target skill's description matches the prompt's intent.
3. Flag gaps — prompts that matched the original but now match nothing.

### Discovery

```bash
npx skills add "<repo-root>" --list
```

Confirm all resulting skills appear. Report issues and offer to fix.

---

## Rules

- **Do not delete originals.** Create new directories alongside originals. The user decides when to remove old versions.
- **Self-contained skills.** Shared references are copied to each directory — no cross-skill file dependencies.
- **Descriptions are synthesized, not concatenated.** Merged descriptions capture unified intent; split descriptions clearly distinguish domains.
- **Always write a manifest.** Every execution produces `migration-manifest.json` for audit and rollback.
- **Name with care.** Domain-based kebab-case over operation-based names (e.g., `design-md` not `create-apply-extract-design`).
