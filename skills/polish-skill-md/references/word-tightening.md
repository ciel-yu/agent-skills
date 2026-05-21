# Word Tightening

Tactical rules for reducing token count in a `SKILL.md` without changing meaning. Apply during the **revise** phase, after structural fixes (pattern alignment, progressive disclosure) are settled. Never tighten before the structure is correct - you will polish prose that should have been deleted.

---

## The Rules

- **Goal**: reduce tokens; improve scanability, parser clarity, and handoff stability.
- **Meaning**: preserve intent, imperatives, constraints, and safety wording; add no semantics.
- **Tokens**: prefer short, common technical terms; use low-token forms when equivalent.
- **Filler**: remove hedges, repetition, connective prose, and weak modifiers.
- **Vocab**: use standard industry terms; avoid rare synonyms.
- **Structure**: prefer headings, bullets, and code blocks; keep hierarchy grouped.
- **Style**: concise, deterministic, agent-readable.

---

## Order of Operations

1. **Structure first.** Convert paragraphs that enumerate to bullet lists. Convert procedural prose to numbered steps. Convert key/value descriptions to tables. Restructuring removes more tokens than wordsmithing ever will.
2. **Filler pass.** Strip hedges, weak modifiers, connective prose. Do not yet touch terminology.
3. **Vocabulary pass.** Replace rare or compound phrases with standard terms.
4. **Final read.** Confirm every imperative, constraint, and safety clause survived intact.

Run passes in this order. Reordering wastes work - a vocabulary swap inside a paragraph you were about to bulletize is throwaway.

---

## Filler to Strip (non-exhaustive)

| Category | Examples | Action |
|---|---|---|
| Hedges | "perhaps", "might want to", "consider", "it may be useful to" | Delete; convert to imperative if load-bearing. |
| Weak modifiers | "very", "really", "quite", "fairly", "somewhat", "simply", "just" | Delete. |
| Connective prose | "It is important to note that", "In order to", "As mentioned above", "Please note" | Delete or replace ("In order to" → "To"). |
| Throat-clearing | "This section describes…", "The following is a list of…", "Below you will find…" | Delete; the heading already announces it. |
| Restated rules | Same constraint repeated in two sections | Keep one; delete the other. |
| Polite framing | "feel free to", "you may want to", "if you'd like" | Delete or convert to imperative. |
| Meta narration | "We will now…", "Next, we will…", "Let's…" | Delete; the structure carries the flow. |

**Soft-gate words are the most dangerous filler in agent-facing files.** "Consider validating" reads as filler to a human but is a real semantic downgrade for an agent: it turns a constraint into a suggestion. Hard-gate constraints (`DO NOT proceed to step N until…`) survive tightening; soft suggestions get deleted in this pass, then the agent silently skips the check. If a hedge is load-bearing, rewrite it as an imperative; if it is not load-bearing, delete the whole sentence.

---

## Worked Transformations

### Restructure prose → bullets

Before (54 tokens):

> The agent should first read the SKILL.md, then enumerate sibling files in references, assets, and scripts directories, and finally note the current size of the file in lines and approximate tokens as well as the front-matter fields.

After (29 tokens):

> 1. Read `SKILL.md` end-to-end.
> 2. List siblings: `references/`, `assets/`, `scripts/`.
> 3. Record size (lines, tokens) and front-matter fields.

### Strip hedges, keep imperative

Before: "You might want to consider making sure that the description is under 1024 characters if possible."
After: "Description: ≤ 1024 chars."

### Strip throat-clearing

Before: "The following is a list of the patterns that this skill supports:"
After: (delete; the bullet list speaks for itself)

### Vocabulary swap

| Before | After |
|---|---|
| "utilize" | "use" |
| "in the event that" | "if" |
| "at this point in time" | "now" |
| "make use of" | "use" |
| "a large number of" | "many" |
| "due to the fact that" | "because" |
| "in order to" | "to" |
| "is able to" | "can" |
| "has the ability to" | "can" |

### Table over key/value prose

Before:

> The `description` field is required and must be no longer than 1024 characters. The `name` field is required and must be in kebab-case.

After:

| Field | Required | Constraint |
|---|---|---|
| `name` | yes | kebab-case |
| `description` | yes | ≤ 1024 chars |

---

## Hard Constraints (never tighten away)

These survive every pass:

- **Imperatives**: "DO NOT", "MUST", "ALWAYS", "NEVER". Tightening these into hedges is a defect, not a polish.
- **Numeric thresholds**: line counts, char limits, timeouts. Keep the number.
- **Negative scope**: "Do not use for…". This is triggering-critical.
- **Gate conditions** in Pipeline skills. See [Gate Test](./patterns.md#5-pipeline).
- **Safety wording** the user added on purpose. When in doubt, preserve and ask.

If a tightening edit removes any of the above, revert it.

---

## When Tightening Is the Wrong Tool

Skip tightening and escalate to a structural revision when:

- SKILL.md is > 500 lines. Word tightening will not save it; apply progressive disclosure first.
- A whole section is generic LLM knowledge (PDF-is-a-format prose). Delete it; do not polish it.
- Sections repeat the same rule. Merge sections; do not tighten both.
- The skill claims a pattern its body does not implement. Fix pattern drift first; tightened drift is still drift.

Tightening is the **last** polish move, not the first.

---

## Verification

After a tightening pass:

- [ ] Token / line count is materially lower.
- [ ] Every imperative, constraint, and numeric threshold from the pre-tightening version still appears.
- [ ] No load-bearing rule was silently converted from imperative to suggestion.
- [ ] No section now starts with throat-clearing prose.
- [ ] Tables, bullets, and code blocks are used wherever they beat prose.
