# Sources

Re-check live sources when a rule affects compatibility or publication.

## Normative and official guidance

- [Agent Skills specification](https://agentskills.io/specification) — required fields, constraints, directories, progressive disclosure, file references, validation
- [Best practices for skill creators](https://agentskills.io/skill-creation/best-practices) — real expertise, context economy, control calibration, defaults, validation loops
- [Optimizing skill descriptions](https://agentskills.io/skill-creation/optimizing-descriptions) — realistic positive and near-miss queries, repeated activation runs, held-out validation
- [Evaluating skill output quality](https://agentskills.io/skill-creation/evaluating-skills) — paired runs, assertions, grading, timing, iteration
- [Using scripts in skills](https://agentskills.io/skill-creation/using-scripts) — dependencies, non-interactive interfaces, errors, structured output
- [Anthropic skill authoring best practices](https://platform.claude.com/docs/en/agents-and-tools/agent-skills/best-practices) — target-client constraints and progressive disclosure
- [Anthropic Agent Skills overview](https://platform.claude.com/docs/en/agents-and-tools/agent-skills/overview) — runtime model and security considerations

## Empirical evidence

- [SkillsBench v4](https://arxiv.org/html/2602.12670) (June 2026) — paired evaluation across 87 tasks and 18 model–harness configurations; focused skills outperform exhaustive bundles
- [SoK: Agentic Skills](https://arxiv.org/html/2602.20867v1) (February 2026) — system-level skill lifecycle, seven infrastructure patterns, and supply-chain risks; it does **not** define the optional five content-design lenses used here

## Optional community vocabulary

- [Lavi Nigam, Five Agent Skill Design Patterns](https://lavinigam.com/posts/adk-skill-design-patterns/) (March 2026) — ADK-oriented Tool Wrapper, Generator, Reviewer, Inversion, and Pipeline vocabulary; useful as a lens, not a specification