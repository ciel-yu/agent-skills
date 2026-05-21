# Repository Instructions

## Purpose

Publish reusable Agent Skills. A valid skill is a directory with a `SKILL.md` manifest, concise instructions, and colocated assets.

## Repository Map

- `skills/<skill-name>/SKILL.md` - required manifest and skill instructions.
- `skills/<skill-name>/references/` - optional deep docs loaded only when needed.
- `README.md` - public overview.
- `.gitignore` - editor, runtime, dependency, log, temp, and build artifacts.

## Default Workflow

When adding or changing a skill:

1. Create or edit `skills/<skill-name>/`; use kebab-case.
2. Keep the entrypoint in `SKILL.md` with YAML front matter: string `name` and `description`.
3. Put concise, tool-oriented instructions in `SKILL.md`.
4. Move narrow or long-form guidance to sibling docs such as `references/*.md`.
5. Keep examples, scripts, and resources inside the skill directory.
6. Verify discovery: `npx skills add "T:\agent-skills" --list`.

## Boundaries

- Do not put skill-specific assets at repo root.
- Do not add a root manifest unless the installer or project requirements explicitly require one; `vercel-labs/skills` discovers valid `SKILL.md` files directly.
- Do not commit generated runtime state, dependency folders, logs, temp files, or build output.
- Preserve skill names unless the user asks to rename them.

## References

- https://agentskills.io/specification
- https://agentskills.io/skill-creation/best-practices
- https://agentskills.io/skill-creation/optimizing-descriptions
- https://agentskills.io/skill-creation/evaluating-skills
- https://agentskills.io/skill-creation/using-scripts
