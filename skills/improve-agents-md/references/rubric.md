# Review Rubric

Use evidence before style. Report only defects that change safety, loading, behavior, cost, or maintainability.

## Severity

### Critical

- invented or unsupported commands, paths, policies, or architecture
- a removed or weakened authoritative safety constraint
- secrets, unsafe setup commands, or instructions enabling unauthorized actions
- wording that falsely claims Markdown enforces a prohibition

### High

- incorrect client discovery, traversal, precedence, or size assumptions
- a nested layout that does not load in the documented workflow
- conflicting parent, child, override, or product-specific instructions
- stale commands or canonical-direction rules likely to cause incorrect changes
- broad instructions placed where they affect unrelated work

### Medium

- generic or inferable content that consumes context without changing behavior
- repository overviews duplicated from maintained documentation
- transient work state presented as durable policy
- exhaustive command lists without one safe default
- useful narrow guidance placed at root instead of its actual scope
- optional links presented as if they auto-load

### Low

- unclear ordering, vague conditions, repeated wording, or inconsistent terminology
- formatting that slows scanning without changing meaning

If nothing fails, return **No findings** and recommend no rewrite.

## Evidence ledger

For every finding or proposed addition, record:

| Item | Evidence | Scope | Decision | Reason |
|---|---|---|---|---|
| section, line, or candidate rule | path, config, command result, or user statement | root / nested / companion / control plane | keep / tighten / relocate / delete / omit | concrete behavior or risk |

Do not convert absence of evidence into a new convention. Ask only when a missing policy is user-owned and materially changes the result.

## Analyze output

Return, in order:

1. target client and verified loading semantics
2. findings by severity, each with evidence
3. classification ledger
4. recommended layout and content outline
5. controls that belong outside Markdown
6. unresolved uncertainty

A no-file or no-change recommendation is a complete result.

## Rewrite checks

- [ ] Only authorized `AGENTS.md` files changed.
- [ ] Every retained or added line has evidence.
- [ ] Every line passes the behavior, scope, and durability tests.
- [ ] Root contains only repository-wide guidance.
- [ ] Nested files load in the intended client workflow.
- [ ] Parent and child guidance is additive or explicitly overriding, never duplicated accidentally.
- [ ] The canonical command and its non-obvious prerequisites remain visible.
- [ ] Expensive or destructive operations retain actionable warnings and safe alternatives.
- [ ] Generic overview, standard advice, copied docs, and work status are absent.
- [ ] Hard requirements are enforced elsewhere or the missing control is reported.
- [ ] Links identify when optional material should be opened.
- [ ] The result is no longer than its demonstrated value requires.

## Anti-churn rules

Do not rewrite solely to:

- impose a preferred heading template
- alphabetize or restyle already clear bullets
- add a repository summary that duplicates the README
- create nested files for symmetry
- convert ordinary prose into RFC keywords without changing policy
- make the file shorter when added evidence genuinely requires more content