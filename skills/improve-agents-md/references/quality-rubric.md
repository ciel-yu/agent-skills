# Quality Rubric

## Rewrite Rubric

Result must satisfy:

- A new contributor or agent can explain what the repo is within 30 seconds.
- The file gives a clear first-pass map.
- The default workflow is obvious.
- When historical patterns conflict, current direction for new work is explicit.
- Task-specific detail is discoverable without root bloat.
- Automated rules are not manual style prose.
- Every section matters across many sessions.

---

## Anti-Patterns to Remove

Remove:

- giant lists of shell commands with no prioritization
- long style-guide sections that belong in tooling
- abstract `Key Principles` lists that do not change decisions
- outdated copied snippets
- instructions for one rare task
- repeated warnings written three different ways
- aspirational guidance with no operational meaning

---

## Final Check

Before finishing, verify:

- the root file is materially shorter or denser than before
- companion docs are linked from the root file
- any `Current Direction` section uses concrete decision rules, paths, and legacy boundaries
- no important global constraint was removed
- the file still reflects the real repository surface
- the result reads like a reusable operator handbook, not prompt dump
