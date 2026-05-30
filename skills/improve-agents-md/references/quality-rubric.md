# Quality Rubric

## Rewrite Rubric

Result must satisfy:

- A new contributor or agent can explain what the repo is within 30 seconds.
- The file gives a clear first-pass map.
- The default workflow is obvious.
- When historical patterns conflict, current direction for new work is explicit.
- All dangerous zones and irreversible operations are captured in `## Boundaries`.
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
- repeated warnings written three different ways (exception: a single, concrete dangerous-zone constraint is not a repeated warning — do not remove or merge it into vague prose)
- aspirational guidance with no operational meaning
- abstract "reasoning framework / chain of thought / cognitive pathway / mental trajectory" sections — if the underlying content is real, reframe it as **thinking discipline**: concrete, observable rules (preconditions, forbidden shortcuts, verification steps), and fold them into Default Workflow / Boundaries / Current Direction / conditional blocks rather than a dedicated section
- a `## Thinking Discipline` (or similarly named) heading: the heading itself is gravity that attracts abstract prose back over time

---

## Final Check

Before finishing, verify:

- the root file is materially shorter or denser than before
- companion docs are linked from the root file
- any `Current Direction` section uses concrete decision rules, paths, and legacy boundaries
- all dangerous zones and irreversible operations are present in `## Boundaries`; none were removed or softened
- no critical fact that prevents silent mistakes was dropped
- the file still reflects the real repository surface
- the result reads like a reusable operator handbook, not prompt dump
