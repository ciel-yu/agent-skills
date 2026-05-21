# Polish Rubric

Apply in severity order. Stop at the first blocking defect, fix it, then re-grade.

---

## Severity Order

1. **Triggering defects** - description will not select the skill when it should, or selects when it should not.
2. **Pattern drift** - declared pattern does not match the body. Cross-check against the [pattern-to-directory cheat sheet](./patterns.md#pattern-to-directory-cheat-sheet): a skill claiming Generator with no `assets/` is drift; a skill claiming Pipeline whose "gates" fail the [Gate Test](./patterns.md#5-pipeline) is drift.
3. **Spec violations** - missing front matter fields, > 1024-char description, SKILL.md > ~500 lines without progressive disclosure.
4. **Context waste** - prose the agent already knows; menu-of-equals; copied snippets that will drift.
5. **Discoverability** - reference files exist but are not linked, or linked without a *when to load* cue.
6. **Style / maintainability** - inconsistent headings, dead links, stale examples, untightened prose (hedges, weak modifiers, throat-clearing, rare-synonym vocabulary). See [word-tightening.md](./word-tightening.md). Tighten only after severities 1-5 are resolved.

---

## Description {#description}

The `description` is the **only** field the agent sees at selection time. Grade it hardest.

**Required**:
- Imperative phrasing: "Use when…" not "This skill does…".
- Frames *user intent*, not internal mechanics.
- Lists concrete trigger phrases / contexts.
- States at least one negative scope ("Do not use for…").
- ≤ 1024 characters.

**Failure modes**:
- *Too narrow* - misses paraphrases ("polish" but not "refine"/"tighten"/"clean up").
- *Too broad* - triggers on adjacent tasks (any markdown edit).
- *Implementation-leaking* - describes how, not when.
- *Keyword-stuffed* - reads like SEO; the agent ignores it.

Fix by generalizing the *category* the failing queries represent, not by appending their exact keywords.

---

## Body

For each section, classify:

- **Keep** - load-bearing for most invocations.
- **Tighten** - keep, but shorter or denser.
- **Split** - move to `references/`, `assets/`, or `scripts/` with an explicit load cue.
- **Delete** - noise, drift bait, or generic LLM knowledge.

Body must answer:

- *When* does this skill apply? (Use When)
- *What* mode is the agent in? (Modes, if any)
- *How* does the agent proceed? (Pipeline / Execution)
- *What* must be true to declare done? (Verification)

---

## Progressive Disclosure

- SKILL.md ≤ ~500 lines / ~5,000 tokens.
- Every reference file is linked **with a when-to-load cue** ("Load if the API returns non-200").
- No reference file is loaded unconditionally - if it is always needed, inline it.
- Scripts > ~30 lines live in `scripts/`, not in SKILL.md fences.

---

## Anti-Patterns (remove on sight)

- "Best practices" prose with no concrete rule.
- Menus of equal options with no default.
- Re-explaining well-known formats (JSON, PDF, HTTP).
- Copied excerpts from external docs that will drift - link instead.
- Conditional blocks with vague conditions ("if you are writing code").
- Multiple warnings restating the same rule.
- Verification sections that only say "make sure it works".
- **Prose gates** in Pipeline skills - soft language ("you might want to validate") that fails the [Gate Test](./patterns.md#5-pipeline). Rewrite as hard constraints ("DO NOT proceed to step N until...").
- **Generation from blank input** - Generator skills that produce artifacts with no real user-provided material. Per the [transformation heuristic](./patterns.md#tool-wrapper-vs-generator-the-ambiguous-case), these read as plausible noise. Require a real input or reclassify as Tool Wrapper.
- **Eager loading** - every reference file linked at the top with no per-phase load cue. Defeats progressive disclosure.
- **Filler and hedges** - "you might want to", "consider", "perhaps", "simply", "just", "in order to". Strip per [word-tightening.md](./word-tightening.md). Soft-gate hedges in Pipeline skills are not style defects, they are pattern drift - escalate to severity 2.
- **Rare-synonym vocabulary** - "utilize" for "use", "leverage" for "use", "in the event that" for "if". Replace with the common term.

---

## Final Check

Before declaring polish done:

- [ ] Description passes the trigger heuristics above.
- [ ] Every section maps to a pattern from [patterns.md](./patterns.md) or is meta (Use When / Verification).
- [ ] SKILL.md is materially shorter or denser than before, without losing load-bearing rules.
- [ ] All companion files are linked with load cues.
- [ ] Reads like a reusable manifest, not a prompt dump.
