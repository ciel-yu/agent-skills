# AGENTS.md

This repository stores reusable Agent Skills.

## Repository layout

- `skills/<skill_name>/SKILL.md` — one skill per directory.
- Optional skill assets (examples/scripts/resources) should stay inside the same `skills/<skill_name>/` directory.

## Skill conventions

To keep skills compatible with the Agent Skills specification:

1. Use `kebab-case` for `<skill_name>` directory names.
2. Each skill directory must contain a `SKILL.md` file.
3. `SKILL.md` should include YAML front matter with at least:
   - `name`
   - `description`
4. Keep instructions actionable, concise, and tool-oriented.
5. Put skill-specific files next to `SKILL.md` instead of the repository root.

## Adding a new skill

1. Create `skills/<skill_name>/`.
2. Add `skills/<skill_name>/SKILL.md` with metadata and usage instructions.
3. Add any supporting files inside the same folder.
