# Optional Design Lenses

These labels are a community-derived vocabulary for discussing skill structure. They are not part of the Agent Skills specification, are not exhaustive, and impose no directory requirements. Use them only when they make a concrete design decision easier to explain.

Source: Lavi Nigam, [5 Agent Skill Design Patterns Every ADK Developer Should Know](https://lavinigam.com/posts/adk-skill-design-patterns/) (2026). The article targets Google ADK and should not be generalized into a cross-client compliance standard.

## Tool Wrapper

Packages non-obvious conventions for a library, service, or internal tool.

Useful when:

- correct use depends on version-specific APIs or local conventions
- the agent otherwise repeats the same discovery work

Possible resources include task-oriented references or tested helper scripts. Neither is mandatory.

## Generator

Produces a repeatable artifact shape from supplied facts or source material.

Useful when:

- output structure is stable
- required inputs and omissions are explicit
- the result has observable acceptance criteria

Short templates may stay inline. Move a template to `assets/` only when size, reuse, or conditional loading justifies it. Pure generation is not automatically defective; grade grounding and evaluation instead.

## Reviewer

Evaluates an artifact against explicit criteria and returns evidence-backed findings.

Useful when:

- criteria are stable and severity has deterministic meaning
- the reviewer can return an explicit no-findings result

A small rubric may stay inline. A larger or independently maintained rubric may live in `references/`.

## Inversion

Collects information only the user or environment can supply before producing an artifact.

Useful when premature execution would create waste or risk. Ask only questions whose answers materially change the result; use defaults for ordinary ambiguity.

## Pipeline

Orders dependent steps and validation gates.

Useful when steps cannot safely be skipped or reordered. A pipeline should state:

- what each gate checks
- what failure does
- which steps are optional
- when a lightweight path is sufficient
- how to recover from a brittle tool or solver

## Composition

A skill may use several lenses, one, or none. Describe composition only when the labels help a maintainer understand execution. Do not add pattern metadata or an introductory label merely for classification symmetry.

## Guardrails

Never use these lenses to:

- decide specification compliance
- require `references/`, `assets/`, or `scripts/`
- force a body template
- manufacture a pattern claim
- delete a useful skill that does not fit the vocabulary

Observed behavior outranks classification. [SkillsBench v4](https://arxiv.org/html/2602.12670) found that focused skills outperform exhaustive bundles and identified three recurring failure modes: heavyweight pipelines crowd out simpler execution, generic guidance displaces a stronger native strategy, and brittle solvers create failures the agent cannot debug. Use applicability boundaries, fast paths, and validation to address those behaviors directly.