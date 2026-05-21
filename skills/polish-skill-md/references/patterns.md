# The Five Skill Design Patterns

Every effective `SKILL.md` consciously implements one or more of these patterns. When polishing, first identify which the skill **claims** and which it **actually** implements - drift is a defect to fix.

The five patterns were popularized by Lavi Nigam ("5 Agent Skill Design Patterns Every ADK Developer Should Know", March 2026) and corroborated by the arXiv survey "SoK: Agentic Skills — Beyond Tool Use in LLM Agents" (February 2026). They are not a taxonomy of skills - they are a vocabulary of *moves* a SKILL.md makes. Most production skills compose 2-3 of them.

Citations and source URLs: [sources.md](./sources.md).

---

## Pattern-to-Directory Cheat Sheet

| Pattern | Uses `references/` | Uses `assets/` | Uses `scripts/` | Complexity |
|---|---|---|---|---|
| Tool Wrapper | Yes (conventions) | - | - | Low |
| Generator | Yes (style guide) | Yes (template) | - | Medium |
| Reviewer | Yes (checklist) | - | Optional (validators) | Medium |
| Inversion | Optional | Yes (output template after Q&A) | - | Medium (multi-turn) |
| Pipeline | Yes | Yes | Optional | High (composes others) |

If a skill claims a pattern but the directory layout does not match this table, that is a drift signal.

---

## 1. Tool Wrapper

**Adapted definition**:
Packages a library or tool's conventions, best practices, and coding standards into on-demand knowledge the agent loads when working with that technology. The simplest `SKILL.md` pattern: instructions plus reference files, no templates or scripts.

**Metaphor**: a cheat sheet for a library that the agent only opens when that library is in play.

**Signals**:
- Skill name reads like "use X" / "work with X" / "X conventions".
- Body is task-grouped recall (authenticate, paginate, stream) plus gotchas.
- Value is *recall*, not *reasoning*.
- Directory: `references/` only.

**Canonical public example**: [`anthropics/skills/claude-api`](https://github.com/anthropics/skills/tree/main/skills/claude-api) - wraps the Anthropic SDK across Python / TS / Java with language-specific reference files. Its description is explicitly gated with both positive triggers ("code imports `anthropic`/`@anthropic-ai/sdk`") and negative scope ("SKIP: file imports `openai`/other-provider SDK").

**Polish rules**:
1. Lead with a one-line "what this wraps" so the agent can confirm scope in a glance.
2. Group `references/` by *task* (authenticate, paginate, stream) - never by alphabetical API surface.
3. Description MUST include both positive triggers (concrete keywords, framework names, file-extension cues) and negative scope. Generic descriptions ("helps with APIs") cause under-triggering and adjacent false positives.
4. Hoist gotchas (counter-intuitive truths the agent will get wrong without being told) into SKILL.md, not `references/` - the agent must see them before encountering the situation.

**Anti-patterns**:
- **Over-generalization** - one skill wrapping FastAPI + Django + Flask. Split into one skill per library.
- **Aspirational conventions** - encoding rules the team doesn't follow. The skill drifts into noise.
- **Alphabetical API dumps** - reference files structured like Sphinx output. Restructure by task.

---

## 2. Generator

**Adapted definition**:
Produces documents, reports, or configurations by filling a reusable template. Uses both optional directories: `assets/` holds the output template; `references/` holds the style guide. Instructions orchestrate: load style guide, load template, gather inputs, fill it in.

**Key insight**: separates **structure** (template in `assets/`) from **quality** (style guide in `references/`). Swapping either file changes the output without touching SKILL.md.

**Signals**:
- Output is a fixed-shape artifact (report, spec, config, manifest).
- The same skeleton repeats across runs.
- Directory: `assets/` + `references/`.

**Canonical public example**: [`anthropics/skills/doc-coauthoring`](https://github.com/anthropics/skills/tree/main/skills/doc-coauthoring) - generates structured documentation. Hybrid with Inversion (gathers context first), but the templated assembly is the Generator move.

**Polish rules**:
1. Templates over ~30 lines belong in `assets/`. Inline only short skeletons in SKILL.md.
2. Every placeholder must declare what fills it, from which data source, and what to omit if data is missing.
3. The style guide in `references/` uses *concrete examples*, not abstract prose. "Active voice. Example: 'The team deployed the service'" beats "Write clearly".

**Anti-patterns**:
- **Template bloat** - a 20-section template with conditional logic is a decision tree, not a template. Split into per-variant skills.
- **Robotic over-constraint** - "every paragraph must be exactly 3 sentences" produces machine-noise output. Constrain shape, not rhythm.
- **Generation from blank input** - empirically the weakest skills are those that produce artifacts from minimal description. Skills that *transform existing material* (closer to Tool Wrapper) hold up better than skills that *generate from nothing*. If your Generator has no real input, reconsider whether the skill earns its keep.

---

## 3. Reviewer

**Adapted definition**:
Evaluates code, content, or artifacts against a checklist stored in `references/`, producing findings grouped by severity. Key design: separate WHAT to check (the checklist file) from HOW to check (the review protocol in instructions).

**Signals**:
- Verbs: review, lint, audit, validate, compare, diff.
- Output is findings ordered by severity, plus optional score.
- Directory: `references/` (checklist), optional `scripts/` (mechanizable checks).

**Polish rules**:
1. The checklist lives in `references/rubric.md` (or similar). Inline only if it's under ~50 lines AND will never be swapped.
2. Organize the checklist by *severity*, with category sub-grouping inside each severity. The agent then prioritizes correctly by default.
3. Every check must be *deterministic*. "Naming is a warning if confusing" is taste; "Naming is a warning if it violates `snake_case` or uses abbreviations > 2 chars" is checkable.
4. Always include an explicit "no findings" branch.
5. If a check is mechanizable, prefer a script in `scripts/` over prose.

**Anti-patterns**:
- **Checklist drift** - the rubric doesn't match what the team enforces. Findings become noise.
- **Subjective severity** - severity that depends on the reviewer's taste rather than the rule. Severity must be a property of the rule, not the reviewer.

---

## 4. Inversion

**Adapted definition**:
Flips the typical agent interaction: the skill asks structured questions through defined phases before producing output. No special framework support; Inversion is instruction-authoring with explicit gates like `DO NOT start building until all phases are complete`.

**Signals**:
- Task hinges on context only the user holds.
- Premature action would waste work or destroy state.
- Phased question lists with hard gates between phases.

**Canonical public example**: [`anthropics/skills/skill-creator`](https://github.com/anthropics/skills/tree/main/skills/skill-creator) - interviews the user about purpose, triggers, output format, and test cases before generating a skill.

**Polish rules**:
1. Use **hard gates**, not soft suggestions. "DO NOT proceed to Phase 2 until all Phase 1 answers are captured" works; "You might want to ask about deployment" does not.
2. Group questions into 3-6 phases with explicit transitions. More than ~6 questions per phase causes user fatigue and the agent starts skipping.
3. Provide sensible defaults the user can accept with one word ("default: Postgres - reply 'ok' to accept").
4. The description must emphasize the *interview* aspect ("Plans X by interviewing the user about Y"), not the output. Otherwise the agent skips straight to producing the output.

**Anti-patterns**:
- **Soft gates** - "You might want to…" / "Consider asking…" - the agent ignores these.
- **Question avalanche** - dumping 20 questions in one phase. Phase them.

---

## 5. Pipeline

**Adapted definition**:
Defines a sequential workflow where each step must complete before the next begins, with explicit gate conditions that prevent skipped validation. The most complex pattern: uses all optional directories and adds control flow between steps.

**Signals**:
- Named, ordered phases with prerequisites or approval points.
- Each phase tends to *be* one of the other four patterns.
- Directory: `references/` + `assets/`, optionally `scripts/`.

**Polish rules**:
1. Draw the pipeline as a single line near the top of SKILL.md. If you can't draw it, it isn't a pipeline.
2. Mark every gate explicitly: *who* decides, *what* unblocks it, *what* happens on failure.
3. Load resources at the step that needs them, not all at the top. This keeps the context lean and the load cues meaningful.
4. Each phase body should be short and point to a reference file for depth.

**The Gate Test** (apply to every claimed gate):
> If you remove the gate sentence, does the agent still follow the order? If yes, it is prose pretending to be a gate. If no, it is a real gate.

**Anti-patterns**:
- **Prose gates** - "make sure to validate before proceeding" - fails the gate test.
- **10+ steps** - debugging and recovery become impossible. Cap at ~6 phases; if you need more, split the skill.
- **Eager loading** - all `references/` files listed at the top with no per-phase load cue. Defeats progressive disclosure.

---

## Composition

Patterns compose. Production skills typically combine 2-3 (corroborated by arXiv SoK, Feb 2026).

Common combinations:

- **Inversion → Generator**: interview the user, then fill a template. (skill-creator, doc-coauthoring)
- **Reviewer + Pipeline**: analyze mode (Reviewer) → rewrite mode (Generator), separated by a gate.
- **Pipeline with embedded Reviewer**: a generation pipeline whose final step grades its own output against a checklist.
- **Generator + Reviewer**: produce an artifact and then audit it before delivering.

**Rule**: name the composition in SKILL.md's intro line ("This skill is a Reviewer + Generator + Pipeline…"). Claimed composition is cheap; matching the body to the claim is load-bearing.

---

## Tool Wrapper vs Generator: the ambiguous case

When a skill both wraps an API *and* emits a structured artifact, classify by the **input**, not the output:

- Input is **existing material** the user provides → Tool Wrapper (a transformation).
- Input is **descriptive intent only** → Generator (a synthesis).

Empirical heuristic (From100to200, May 2026, on a 20-skill review): *"The skills that hold up are transformations of existing material. The ones that generate from a blank input read as plausible noise."* Prefer pulling ambiguous skills toward the Tool Wrapper side; require a real input.

---

## If you cannot name a pattern

If a SKILL.md you are polishing implements *none* of these five patterns, it is almost certainly advice prose dressed as a skill. Three options:

1. Reframe it - is it actually a Tool Wrapper for a domain you didn't notice?
2. Split it into a real skill plus a `references/*.md` doc.
3. Delete it.

Claims are cheap. Bodies are load-bearing. Fix the body.
