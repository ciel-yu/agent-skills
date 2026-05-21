# Sources

Primary and secondary sources used in [patterns.md](./patterns.md) and [rubric.md](./rubric.md). Pinned dates where possible.

## Primary

1. **agentskills.io - Specification** - the authoritative format spec; defines `name`, `description`, progressive disclosure, the ≤500-line / ≤5,000-token target, and the `references/` / `assets/` / `scripts/` directory conventions. <https://agentskills.io/specification>
2. **agentskills.io - Best practices for skill creators** - "add what the agent lacks, omit what it knows", gotchas sections, validation loops, plan-validate-execute. <https://agentskills.io/skill-creation/best-practices>
3. **agentskills.io - Optimizing skill descriptions** - description triggering, imperative phrasing, train/validation split for description eval, 1024-char limit. <https://agentskills.io/skill-creation/optimizing-descriptions>
4. **Anthropic, "Introducing Agent Skills"** (Oct 16, 2025) - launch post; framing of skills as composable, progressively-loaded capabilities. <https://www.anthropic.com/index/skills>
5. **Anthropic, "The Complete Guide to Building Skills for Claude"** (Jan 2026) - long-form authoring guide. <https://resources.anthropic.com/hubfs/The-Complete-Guide-to-Building-Skill-for-Claude.pdf>
6. **anthropics/skills** (GitHub) - reference implementations. Notable: `claude-api` (Tool Wrapper), `doc-coauthoring` (Inversion + Generator), `skill-creator` (Inversion). <https://github.com/anthropics/skills>

## Pattern Vocabulary

7. **Lavi Nigam, "5 Agent Skill Design Patterns Every ADK Developer Should Know"** (March 7, 2026) - the canonical naming of Tool Wrapper / Generator / Reviewer / Inversion / Pipeline. Source of the pattern vocabulary and definition language adapted in `patterns.md`. <https://lavinigam.com/posts/adk-skill-design-patterns/>
8. **arXiv, "SoK: Agentic Skills — Beyond Tool Use in LLM Agents"** (Feb 2026) - systems-level survey corroborating that production skills typically combine 2-3 patterns; identifies metadata-driven progressive disclosure as the most-deployed mechanism. <https://arxiv.org/html/2602.20867v1>

## Secondary / Field Reports

9. **From100to200, "Transformations vs generators: a pattern from 20 Claude Skills"** (May 10, 2026) - empirical finding that transformation-style skills outperform pure generators. Source of the input-based disambiguation heuristic. <https://dev.to/from100to200/transformations-vs-generators-a-pattern-from-20-claude-skills-4no5>
10. **Patrick Hughes, "MCP vs Skills: a practical decision guide for builders"** (May 4, 2026) - decision heuristics for when a wrapper should be an MCP server vs a Skill. <https://bmdpat.com/blog/mcp-vs-skills-decision-guide-2026>
11. **runkids/skillshare** (GitHub) - community reference implementations with explicit `pattern:` front-matter field. Useful as worked examples of Reviewer, Inversion, Pipeline. <https://github.com/runkids/skillshare>
