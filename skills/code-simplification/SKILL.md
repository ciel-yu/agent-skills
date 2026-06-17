---
name: code-simplification
description: 'Simplify code for clarity without changing behavior. Use when refactoring code that works but is harder to read, maintain, or extend than it should be — deep nesting, long functions, duplicated logic, unclear names, accumulated complexity from rushed changes. Language-agnostic: applies to any programming language, framework, or paradigm. Do not use for new code generation, performance-critical hot paths where the clearer version is measurably slower, or code you don''t yet understand.'
---

# Code Simplification

> Inspired by the [Claude Code Simplifier plugin](https://github.com/anthropics/claude-plugins-official/blob/main/plugins/code-simplifier/agents/code-simplifier.md). Adapted here as a language-agnostic and model-agnostic, process-driven skill for any AI coding agent working in any language.

This skill is a **Pipeline with an embedded Reviewer**: `understand → identify → apply → verify`. It produces simplified code, then audits the result against a checklist. The principles below are universal — they make no assumption about the programming language, framework, or paradigm. Code examples here use language-neutral pseudocode; for idiomatic before/after examples in a specific language, load [references/language-idioms.md](./references/language-idioms.md) on demand.

## Overview

Simplify code by reducing complexity while preserving exact behavior. The goal is not fewer lines — it's code that is easier to read, understand, modify, and debug. Every simplification must pass one test: "Would a new team member understand this faster than the original?"

## When to Use

Use when:

- A feature works and tests pass, but the implementation feels heavier than it needs to be
- Reviewing code that has accumulated unnecessary complexity
- You encounter deeply nested logic, long functions, or unclear names
- Refactoring code written under time pressure
- Consolidating related logic scattered across files
- After merging changes that introduced duplication or inconsistency

Do **not** use for:

- Code that is already clean and readable — don't simplify for the sake of it
- Code you don't understand yet — comprehend before you simplify
- Performance-critical code where the clearer version is measurably slower
- A module you're about to rewrite entirely — simplifying throwaway code wastes effort

## The Five Principles

### 1. Preserve Behavior Exactly

Don't change what the code does — only how it expresses it. All inputs, outputs, side effects, error behavior, and edge cases must remain identical. If you're not sure a simplification preserves behavior, don't make it.

```
ASK BEFORE EVERY CHANGE:
→ Does this produce the same output for every input?
→ Does this maintain the same error behavior?
→ Does this preserve the same side effects and ordering?
→ Do all existing tests still pass without modification?
```

### 2. Follow Project Conventions

Simplification means making code more consistent with the codebase, not imposing external preferences. Before simplifying:

```
1. Read the project's conventions file (AGENTS.md / CLAUDE.md / CONTRIBUTING)
2. Study how neighboring code handles similar patterns
3. Match the project's style for:
   - Import / module ordering and module system
   - Function declaration style
   - Naming conventions
   - Error handling patterns
   - Type annotation depth (in languages with types)
```

Simplification that breaks project consistency is not simplification — it's churn.

### 3. Prefer Clarity Over Cleverness

Explicit code is better than compact code when the compact version requires a mental pause to parse. The examples below use language-neutral pseudocode; the patterns hold in any language.

```
UNCLEAR — a chained conditional expression forces a mental stack:
    label = isNew ? "New" : isUpdated ? "Updated" : isArchived ? "Archived" : "Active"

CLEAR — a named function with early returns:
    function statusLabel(item):
        if item.isNew:      return "New"
        if item.isUpdated:  return "Updated"
        if item.isArchived: return "Archived"
        return "Active"
```

```
UNCLEAR — a clever one-line fold hides the intent:
    counts = items.fold(empty map, (acc, item) -> acc[item.id] += 1)

CLEAR — an explicit loop with a named result:
    counts = empty map
    for item in items:
        counts[item.id] = counts[item.id] + 1
```

### 4. Maintain Balance

Simplification has a failure mode: over-simplification. Watch for these traps:

- **Inlining too aggressively** — removing a helper that gave a concept a name makes the call site harder to read
- **Combining unrelated logic** — two simple functions merged into one complex function is not simpler
- **Removing "unnecessary" abstraction** — some abstractions exist for extensibility or testability, not complexity
- **Optimizing for line count** — fewer lines is not the goal; easier comprehension is

### 5. Scope to What Changed

Default to simplifying recently modified code. Avoid drive-by refactors of unrelated code unless explicitly asked to broaden scope. Unscoped simplification creates noise in diffs and risks unintended regressions.

## The Simplification Process

`understand → identify ──[GATE: can't answer why it exists? keep reading]──▶ apply → verify`

### Step 1: Understand Before Touching (Chesterton's Fence)

Before changing or removing anything, understand why it exists. If you see a fence across a road and don't understand why it's there, don't tear it down. First understand the reason, then decide if the reason still applies.

```
BEFORE SIMPLIFYING, ANSWER:
- What is this code's responsibility?
- What calls it? What does it call?
- What are the edge cases and error paths?
- Are there tests that define the expected behavior?
- Why might it have been written this way? (Performance? Platform constraint? Historical reason?)
- Check version history (git blame or equivalent): what was the original context?
```

**Gate:** if you can't answer these, you're not ready to simplify. Read more context first.

### Step 2: Identify Simplification Opportunities

Scan for these patterns — each is a concrete signal, not a vague smell. The signals are language-neutral; the idiomatic fix varies by language.

**Structural complexity:**

| Pattern | Signal | Simplification |
|---------|--------|----------------|
| Deep nesting (3+ levels) | Hard to follow control flow | Extract conditions into guard clauses or helper functions |
| Long functions (50+ lines) | Multiple responsibilities | Split into focused functions with descriptive names |
| Chained conditional expressions (e.g. nested ternaries) | Requires a mental stack to parse | Replace with if/else chains, switch/match, or a lookup table |
| Boolean parameter flags | `doThing(true, false, true)` | Replace with named parameters/options or split into separate functions |
| Repeated conditionals | The same check in multiple places | Extract to a well-named predicate function |

**Naming and readability:**

| Pattern | Signal | Simplification |
|---------|--------|----------------|
| Generic names | `data`, `result`, `temp`, `val`, `item` | Rename to describe the content: `userProfile`, `validationErrors` |
| Abbreviated names | `usr`, `cfg`, `btn`, `evt` | Use full words unless the abbreviation is universal (`id`, `url`, `api`) |
| Misleading names | A function named `get` that also mutates state | Rename to reflect actual behavior |
| Comments explaining "what" | `// increment counter` next to `count = count + 1` | Delete the comment — the code is clear enough |
| Comments explaining "why" | `// retry because the API is flaky under load` | Keep these — they carry intent the code can't express |

**Redundancy:**

| Pattern | Signal | Simplification |
|---------|--------|----------------|
| Duplicated logic | The same 5+ lines in multiple places | Extract to a shared function |
| Dead code | Unreachable branches, unused variables, commented-out blocks | Remove (after confirming it's truly dead) |
| Unnecessary abstractions | A wrapper that adds no value | Inline the wrapper; call the underlying function directly |
| Over-engineered patterns | Factory-for-a-factory, strategy-with-one-strategy | Replace with the simple direct approach |
| Redundant annotations or casts | A type annotation/cast the compiler can already infer | Remove the redundant annotation/cast |

> For concrete before/after examples in a specific language, see [references/language-idioms.md](./references/language-idioms.md). Load it **only** when the target code is written in one of the languages it covers.

### Step 3: Apply Changes Incrementally

Make one simplification at a time. Run tests after each change. **Submit refactoring changes separately from feature or bug fix changes.** A PR that refactors and adds a feature is two PRs — split them.

```
FOR EACH SIMPLIFICATION:
1. Make the change
2. Run the test suite
3. If tests pass → commit (or continue to the next simplification)
4. If tests fail → revert and reconsider
```

Avoid batching multiple simplifications into one untested change. If something breaks, you need to know which simplification caused it.

**The Rule of 500:** if a refactoring would touch more than 500 lines, invest in scripted edits (AST transforms, find-and-replace scripts, or language-specific codemods) rather than making the changes by hand. Manual edits at that scale are error-prone and exhausting to review.

### Step 4: Verify the Result

After all simplifications, step back and evaluate the whole:

```
COMPARE BEFORE AND AFTER:
- Is the simplified version genuinely easier to understand?
- Did you introduce any patterns inconsistent with the codebase?
- Is the diff clean and reviewable?
- Would a teammate approve this change?
```

If the "simplified" version is harder to understand or review, revert. Not every simplification attempt succeeds.

## Common Rationalizations

| Rationalization | Reality |
|---|---|
| "It's working, no need to touch it" | Working code that's hard to read will be hard to fix when it breaks. Simplifying now saves time on every future change. |
| "Fewer lines is always simpler" | A one-line nested conditional (ternary chain) is not simpler than a five-line if/else. Simplicity is about comprehension speed, not line count. |
| "I'll just quickly simplify this unrelated code too" | Unscoped simplification creates noisy diffs and risks regressions in code you didn't intend to change. Stay focused. |
| "The types make it self-documenting" | Types document structure, not intent. A well-named function explains *why* better than a type signature explains *what*. (Applies to typed languages.) |
| "This abstraction might be useful later" | Don't preserve speculative abstractions. If it's not used now, it's complexity without value. Remove it and re-add when needed. |
| "The original author must have had a reason" | Maybe. Check version history — apply Chesterton's Fence. But accumulated complexity often has no reason; it's just the residue of iteration under pressure. |
| "I'll refactor while adding this feature" | Separate refactoring from feature work. Mixed changes are harder to review, revert, and understand in history. |

## Red Flags

- Simplification that requires modifying tests to pass (you likely changed behavior)
- "Simplified" code that is longer and harder to follow than the original
- Renaming things to match your preferences rather than project conventions
- Removing error handling because "it makes the code cleaner"
- Simplifying code you don't fully understand
- Batching many simplifications into one large, hard-to-review commit
- Refactoring code outside the scope of the current task without being asked

## Verification

After completing a simplification pass:

- [ ] All existing tests pass without modification
- [ ] Build succeeds with no new warnings
- [ ] Linter/formatter passes (no style regressions)
- [ ] Each simplification is a reviewable, incremental change
- [ ] The diff is clean — no unrelated changes mixed in
- [ ] Simplified code follows project conventions (checked against AGENTS.md / CLAUDE.md / contributing guide)
- [ ] No error handling was removed or weakened
- [ ] No dead code was left behind (unused imports, unreachable branches)
- [ ] A teammate or review agent would approve the change as a net improvement

---

A good simplification reads as obvious in hindsight: behavior unchanged, intent clearer, diff small.
