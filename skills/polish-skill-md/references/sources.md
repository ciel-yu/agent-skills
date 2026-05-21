# Sources

Sources cited in [patterns.md](./patterns.md) and [rubric.md](./rubric.md). Dates pinned where possible.

## Primary

1. **agentskills.io - Specification** - format spec for `name`, `description`, progressive disclosure, the ≤500-line / ≤5,000-token target, and `references/` / `assets/` / `scripts/` conventions. <https://agentskills.io/specification>
2. **agentskills.io - Best practices for skill creators** - "add what the agent lacks, omit what it knows", gotchas sections, validation loops, plan-validate-execute. <https://agentskills.io/skill-creation/best-practices>
3. **agentskills.io - Optimizing skill descriptions** - description triggering, imperative phrasing, train/validation splits for description eval, 1024-char limit. <https://agentskills.io/skill-creation/optimizing-descriptions>
4. **Anthropic, "Introducing Agent Skills"** (Oct 16, 2025) - launch post framing skills as composable, progressively loaded capabilities. <https://www.anthropic.com/index/skills>
5. **Anthropic, "The Complete Guide to Building Skills for Claude"** (Jan 2026) - long-form authoring guide. <https://resources.anthropic.com/hubfs/The-Complete-Guide-to-Building-Skill-for-Claude.pdf>
6. **anthropics/skills** (GitHub) - reference implementations. Notable: `claude-api` (Tool Wrapper), `doc-coauthoring` (Inversion + Generator), `skill-creator` (Inversion). <https://github.com/anthropics/skills>

## Pattern Vocabulary

7. **Lavi Nigam, "5 Agent Skill Design Patterns Every ADK Developer Should Know"** (March 7, 2026) - canonical naming for Tool Wrapper / Generator / Reviewer / Inversion / Pipeline. Source for the adapted pattern vocabulary in `patterns.md`. <https://lavinigam.com/posts/adk-skill-design-patterns/>
8. **arXiv, "SoK: Agentic Skills — Beyond Tool Use in LLM Agents"** (Feb 2026) - survey supporting that production skills usually combine 2-3 patterns and use metadata-driven progressive disclosure. <https://arxiv.org/html/2602.20867v1>

## Secondary / Field Reports

9. **From100to200, "Transformations vs generators: a pattern from 20 Claude Skills"** (May 10, 2026) - field report that transformation-style skills outperform pure generators. Source for the input-based disambiguation heuristic. <https://dev.to/from100to200/transformations-vs-generators-a-pattern-from-20-claude-skills-4no5>
10. **Patrick Hughes, "MCP vs Skills: a practical decision guide for builders"** (May 4, 2026) - heuristics for deciding when a wrapper should be an MCP server versus a Skill. <https://bmdpat.com/blog/mcp-vs-skills-decision-guide-2026>
11. **runkids/skillshare** (GitHub) - community skill examples with explicit `pattern:` front matter; useful Reviewer, Inversion, and Pipeline samples. <https://github.com/runkids/skillshare>
