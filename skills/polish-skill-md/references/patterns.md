# The Five Skill Design Patterns

Every effective `SKILL.md` implements one or more of these patterns. First compare the pattern(s) a skill **claims** with the pattern(s) it **actually** implements. Drift is a defect.

Lavi Nigam popularized the five-pattern vocabulary in "5 Agent Skill Design Patterns Every ADK Developer Should Know" (March 2026). The arXiv survey "SoK: Agentic Skills — Beyond Tool Use in LLM Agents" (February 2026) corroborates the model. These are not a taxonomy; they are a vocabulary of *moves* a `SKILL.md` makes. Most production skills combine 2-3.

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

If a skill claims a pattern but its directory layout does not match this table, treat that as drift.

---

## 1. Tool Wrapper

**Adapted definition**:
Packages a library or tool's conventions into on-demand guidance. Simplest pattern: instructions plus `references/`, no templates or scripts.

**Metaphor**: a cheat sheet the agent opens only when that library is in play.

**Signals**:
- Name reads like "use X" / "work with X" / "X conventions".
- Body groups recall by task (authenticate, paginate, stream) plus gotchas.
- Value is *recall*, not *reasoning*.
- Directory: `references/` only.

**Canonical public example**: [`anthropics/skills/claude-api`](https://github.com/anthropics/skills/tree/main/skills/claude-api) wraps the Anthropic SDK across Python / TS / Java with language-specific reference files. Its description includes both positive triggers (imports `anthropic` / `@anthropic-ai/sdk`) and negative scope (`openai` / other providers).

**Polish rules**:
1. Lead with one line stating what the skill wraps.
2. Group `references/` by *task*, not alphabetical API surface.
3. Description MUST include concrete positive triggers and negative scope.
4. Hoist gotchas into `SKILL.md`, not `references/`.

**Anti-patterns**:
- **Over-generalization** - one skill wrapping FastAPI + Django + Flask. Split by library.
- **Aspirational conventions** - rules the team does not follow. They become noise.
- **Alphabetical API dumps** - docs organized like generated reference output. Restructure by task.

---

## 2. Generator

**Adapted definition**:
Produces fixed-shape artifacts from a reusable template. `assets/` holds the template; `references/` holds the style guide.

**Key insight**: separate **structure** (`assets/`) from **quality** (`references/`). You can swap either without rewriting `SKILL.md`.

**Signals**:
- Output is a fixed-shape artifact (report, spec, config, manifest).
- The same skeleton repeats across runs.
- Directory: `assets/` + `references/`.

**Canonical public example**: [`anthropics/skills/doc-coauthoring`](https://github.com/anthropics/skills/tree/main/skills/doc-coauthoring) generates structured documentation. It is hybrid Inversion + Generator, but the templated assembly is the Generator move.

**Polish rules**:
1. Templates over ~30 lines belong in `assets/`.
2. Every placeholder must say what fills it, from which source, and what to omit if data is missing.
3. Style guides in `references/` need concrete examples, not abstract prose.

**Anti-patterns**:
- **Template bloat** - a 20-section template with branching logic is a decision tree. Split it.
- **Robotic over-constraint** - rules like "every paragraph must be exactly 3 sentences" create machine-noise output.
- **Generation from blank input** - pure synthesis is weaker than transforming real material. If a Generator has no real input, reconsider it.

---

## 3. Reviewer

**Adapted definition**:
Evaluates code, content, or artifacts against a checklist in `references/`, then returns findings by severity. Separate WHAT to check from HOW to check it.

**Signals**:
- Verbs: review, lint, audit, validate, compare, diff.
- Output is findings ordered by severity, plus optional score.
- Directory: `references/` (checklist), optional `scripts/` (mechanizable checks).

**Polish rules**:
1. Put the checklist in `references/rubric.md` (or similar). Inline only if it is tiny and fixed.
2. Organize the checklist by *severity*, then category.
3. Every check must be *deterministic*.
4. Always include an explicit "no findings" branch.
5. If a check is mechanizable, prefer a script over prose.

**Anti-patterns**:
- **Checklist drift** - the rubric no longer matches team standards.
- **Subjective severity** - severity changes with reviewer taste instead of the rule.

---

## 4. Inversion

**Adapted definition**:
Flips the usual agent flow: the skill asks structured questions in phases before producing output. Inversion is instruction design with explicit gates like `DO NOT start building until all phases are complete`.

**Signals**:
- The task depends on context only the user has.
- Premature action would waste work or destroy state.
- Questions are phased and separated by hard gates.

**Canonical public example**: [`anthropics/skills/skill-creator`](https://github.com/anthropics/skills/tree/main/skills/skill-creator) interviews the user about purpose, triggers, output format, and test cases before generating a skill.

**Polish rules**:
1. Use **hard gates**, not suggestions.
2. Group questions into 3-6 phases with explicit transitions.
3. Provide sensible defaults the user can accept in one word.
4. Description must foreground the *interview*, not just the output.

**Anti-patterns**:
- **Soft gates** - "You might want to…" / "Consider asking…".
- **Question avalanche** - 20 questions in one phase.

---

## 5. Pipeline

**Adapted definition**:
Defines a sequential workflow whose steps must complete in order, with explicit gates that block skipped validation. It is the most complex pattern and often composes the other four.

**Signals**:
- Named, ordered phases with prerequisites or approval points.
- Each phase tends to *be* one of the other four patterns.
- Directory: `references/` + `assets/`, optionally `scripts/`.

**Polish rules**:
1. Draw the pipeline as a single line near the top of `SKILL.md`.
2. Mark every gate explicitly: *who* decides, *what* unblocks it, *what* failure does.
3. Load resources at the step that needs them, not all at the top.
4. Keep each phase short and point to a reference file for depth.

**The Gate Test** (apply to every claimed gate):
> If you remove the gate sentence and the agent still follows the order, it was prose pretending to be a gate. If not, it was real.

**Anti-patterns**:
- **Prose gates** - "make sure to validate before proceeding". Fails the Gate Test.
- **10+ steps** - recovery becomes hard. Cap at ~6 steps; split if you need more.
- **Eager loading** - all `references/` files listed up front with no per-phase cue.

---

## Composition

Patterns compose. Most production skills combine 2-3.

Common combinations:

- **Inversion → Generator**: interview the user, then fill a template. (`skill-creator`, `doc-coauthoring`)
- **Reviewer + Pipeline**: analyze mode (Reviewer) → rewrite mode (Generator), separated by a gate.
- **Pipeline with embedded Reviewer**: a generation pipeline whose final step audits its own output.
- **Generator + Reviewer**: produce an artifact, then audit it before delivery.

**Rule**: name the composition in the `SKILL.md` intro line (for example, "This skill is a Reviewer + Generator + Pipeline..."). Claims are cheap; the body must match.

---

## Tool Wrapper vs Generator: the ambiguous case

When a skill both wraps an API *and* emits a structured artifact, classify by the **input**, not the output:

- Input is **existing material** the user provides → Tool Wrapper (transformation).
- Input is **descriptive intent only** → Generator (synthesis).

Empirical heuristic (From100to200, May 2026, on a 20-skill review): *"The skills that hold up are transformations of existing material. The ones that generate from a blank input read as plausible noise."* When a case is ambiguous, pull it toward Tool Wrapper and require real input.

---

## If you cannot name a pattern

If a `SKILL.md` you are polishing fits *none* of these patterns, it is probably advice prose dressed as a skill. Three options:

1. Reframe it - maybe it is a Tool Wrapper for a domain you missed.
2. Split it into a real skill plus a `references/*.md` document.
3. Delete it.

Claims are cheap. Bodies are load-bearing. Fix the body.
