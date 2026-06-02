# Split Heuristics

Identify where a skill should split and validate the resulting boundaries. Load this file during the **analyze** and **plan** steps of a split.

---

## Identifying Split Points

### Signal 1: Independent Concerns

A skill addresses two or more concerns that an agent would invoke independently.

**Detection**:
- Scan `SKILL.md` for top-level headings (`##`). If headings naturally cluster into 2-3 groups that don't reference each other, each group is a split candidate.
- Check the "Use When" section. If it lists trigger contexts spanning unrelated user intents, those intents may belong in separate skills.
- Check companion files. If `references/` partitions cleanly (some files serve concern A, others serve concern B, minimal overlap), that confirms the boundary.

**Confidence rating**:
- **Clear** — two completely independent concern clusters with zero shared companion files.
- **Possible** — clusters exist but share 1-2 companion files.
- **Speculative** — clusters are fuzzy or share most companion files.

### Signal 2: Size Pressure

The skill is too large for a single load.

**Thresholds**:
- `SKILL.md` over ~400 lines → investigate split.
- More than 6 companion files → investigate redistribution.
- `SKILL.md` has sections that are effectively self-contained sub-skills (own pipeline, own verification) → strong split signal.

**Note**: size alone is not sufficient reason to split. A Pipeline skill with 6 phases is legitimately large. Split only when sections also show independent concerns.

### Signal 3: Triggering Conflict

The `description` tries to serve too many masters.

**Symptoms**:
- The "Do not use for..." list is longer than the positive triggers.
- The description uses "or" to join unrelated trigger contexts more than twice.
- The description exceeds 800 characters despite being well-written (dense, not verbose).
- Testing reveals the skill triggers incorrectly — activates for intent A when the user meant intent B.

---

## Validating Split Boundaries

After identifying candidate split points, validate each boundary:

### Cohesion Check

For each proposed sub-skill, verify:

1. It has its own clear trigger context (the user would ask for this independently).
2. It has its own pipeline or workflow (not just a fragment of the original's pipeline).
3. It can be described in ≤ 1024 chars without referencing the other sub-skills.

If a candidate fails any check, merge it back into the closest sibling or keep it in the original.

### Coupling Check

For each pair of proposed sub-skills, count:

- Shared companion files (files both sub-skills reference).
- Cross-references in `SKILL.md` body (one sub-skill's sections link to another's sections).
- Shared pipeline steps (both sub-skills invoke the same phase logic).

**Interpretation**:
- 0 shared items → clean split, low risk.
- 1-2 shared items → copy the shared files to both directories (skills must be self-contained).
- 3+ shared items → reconsider the boundary. High coupling suggests the split may produce skills that are hard to maintain independently.

### Description Discrimination Check

After drafting descriptions for each sub-skill:

1. Verify no two sub-skills share the same primary trigger phrase.
2. Verify each sub-skill's "Do not use for" section explicitly excludes the other sub-skills.
3. Check that the descriptions together cover the original skill's full trigger surface — no gaps where a previously-handled user intent now falls through.

---

## Companion File Redistribution

For each companion file in the original skill:

### Ownership Assignment

| Condition | Action |
|---|---|
| File is referenced only from sections going to sub-skill A | Move to A |
| File is referenced from sections in both A and B | Copy to both |
| File is not referenced from any section | Flag as orphan; ask user whether to keep, move, or delete |
| File is a template or asset used only by one sub-skill's workflow | Move to that sub-skill |

### Shared Files

When a file must be copied to multiple sub-skills:

1. Copy the full file. Do not slice it.
2. Each copy lives independently — future edits to one do not affect the other.
3. Consider whether the shared file should instead be split along the same boundary. If its sections clearly serve different sub-skills, split the file too.
4. Record the decision in the plan so the user can audit.

---

## Reverse Trigger Testing

After writing the new sub-skills, validate that they cover the original's trigger surface.

**Procedure**:

1. Extract 3-5 representative user prompts from the original skill's "Use When" section and description triggers. These are prompts the original skill should have caught.
2. For each prompt, determine which new sub-skill should handle it.
3. Verify the target sub-skill's description actually matches the prompt's intent.
4. Check for gaps: prompts that would have triggered the original but now match none of the sub-skills.

**Example**:

```
Original skill: design-md (create + extract + apply)
Prompt: "I have an existing project, pull out the design tokens into a DESIGN.md"
Expected target: extract-design-md
Result: ✓ extract-design-md description includes "extract DESIGN.md from existing project"

Prompt: "Apply the design system from DESIGN.md to these components"
Expected target: apply-design-md
Result: ✓ apply-design-md description includes "implementing UI from DESIGN.md"

Prompt: "I need to create a design system for a new project"
Expected target: create-design-md
Result: ✓ create-design-md description includes "creating DESIGN.md"
```

If any prompt falls through, refine the relevant sub-skill's description or reconsider the boundary.
