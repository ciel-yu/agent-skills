# Merge Strategies

When companion files from different skills collide, apply the correct strategy. Load this file during the **analyze** and **plan** steps of a merge.

---

## Strategy Selection

| Condition | Strategy |
|---|---|
| Files are byte-identical or trivially different (whitespace, formatting) | **Keep** |
| Files cover the same topic but one is strictly richer (more sections, more rules, stricter checks) | **Keep stronger** |
| Files cover overlapping topics with unique material in each | **Merge** |
| One file is a strict subset of another | **Demote** — keep the superset, move the subset's unique lines into it and discard the rest |
| Files are unrelated despite similar names | **Rename** — disambiguate by prefixing the domain (e.g., `scoring-rubric.md` → `design-scoring-rubric.md` vs `code-scoring-rubric.md`) |

When uncertain between **Keep stronger** and **Merge**, default to **Merge** — it preserves more material.

---

## Detailed Strategies

### 1. Keep

The simplest case. Two skills include the same file (e.g., a shared `equivalence-checks.md`).

**Rules**:
- Copy one copy. Do not duplicate.
- Verify the surviving file's internal links still resolve in the merged directory.
- If the file links to other companion files, check those links still exist post-merge.

### 2. Keep Stronger

Two files address the same topic but differ in depth or strictness.

**How to judge "stronger"**:
- More concrete rules > fewer concrete rules.
- Includes examples > no examples.
- Numeric thresholds > vague language ("appropriate", "reasonable").
- Covers edge cases > ignores them.
- Has load cues and structure > flat prose.

**Rules**:
- Keep the stronger file as-is.
- Scan the weaker file for any unique rules or examples not present in the stronger one. If found, promote them into the stronger file rather than discarding.
- Record the decision in the plan so the user can audit.

### 3. Merge

Both files have unique, load-bearing content covering the same domain.

**Rules**:
- Structure the merged file with clear section headers. Group by concern, not by source skill.
- Do not interleave — keep each source's material coherent within its section.
- Add a short intro paragraph stating the file's scope.
- Deduplicate: if both files state the same rule, keep the clearer formulation (not both).
- Resolve conflicts: when rules contradict, flag in the plan for user decision. Do not silently pick one.
- Keep the merged file under ~300 lines. If it exceeds this, split into two focused files and link from `SKILL.md`.

### 4. Demote

One file is a strict subset of another.

**Rules**:
- Merge the subset's unique lines into the superset file.
- Delete the subset file.
- Update any `SKILL.md` links that pointed to the subset file to point to the superset.

### 5. Rename

Files with similar names cover different domains.

**Rules**:
- Prefix with the domain or sub-problem name.
- Keep names descriptive but concise: `review-scoring-rubric.md`, not `rubric-for-review-phase.md`.
- Update all internal links in `SKILL.md` and other companion files.

---

## Cross-Reference Heat Map

Before merging, build a cross-reference map to detect hidden dependencies:

```
                    skill-a          skill-b
                    ─────────        ─────────
references/X.md     ✓ links → Z     —
references/Y.md     —               ✓ links → Z
references/Z.md     ✓               ✓
references/W.md     ✓               —
```

For each shared file:
1. Count how many other companion files link to it (from both skills).
2. Files with high inbound links from both skills are **merge-critical** — handle them with the **Merge** strategy even if they appear similar, because changes ripple.
3. Files linked only from one skill are safe to **Keep** or **Keep stronger**.

Present this heat map in the plan so the user sees dependency density at a glance.

---

## Description Conflict Detection

After drafting the merged `description`, verify it does not collide with descriptions of *unrelated* skills in the same repository.

**Procedure**:
1. Read the `description` field of every other skill in the repo (via `npx skills add <repo-root> --list` or by scanning sibling `SKILL.md` files).
2. Extract positive trigger phrases from each description (the "Use when..." / "Triggers:..." parts).
3. Compare the merged draft's positive triggers against every other skill's positive triggers.
4. Flag any pair where:
   - Two skills share ≥ 3 identical or near-identical trigger phrases, **or**
   - One skill's positive trigger is another's negative scope ("Do not use for...").
5. If conflicts exist, refine the merged description to narrow its scope or add a discriminating negative boundary.

Report conflicts in the plan. Example:

```
⚠ Trigger overlap with 'polish-skill-md':
  Both mention "rewrite SKILL.md".
  Resolution: merged skill uses "restructure" / "merge" / "split";
  polish-skill-md retains "polish" / "tighten" / "lint".
```
