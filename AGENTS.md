# Repository Instructions

## Skill changes

- Write skills in English under `skills/<skill-name>/`; use kebab-case. Preserve existing skill names unless the user requests a rename.
- Keep `SKILL.md` concise and tool-oriented, with YAML string fields `name` (matching the directory name) and `description`. Move narrow or long-form guidance to sibling docs such as `references/*.md`.
- Keep skill-specific examples, scripts, templates, and other assets inside the owning skill directory, not at repo root.
- Do not add a repo-root manifest unless the installer or project requirements explicitly require one; `vercel-labs/skills` discovers valid `SKILL.md` files directly.
- Do not commit generated runtime state, dependency folders, logs, temp files, or build output.

## Verify discovery

After adding or changing a skill, run from the repository root:

```sh
npx skills add . --list
```

Confirm every affected skill appears under its expected name.

## Task-specific guidance

- When changing manifest metadata or structure, consult the [Agent Skills specification](https://agentskills.io/specification).
- For authoring, description tuning, evaluations, or scripts, use the corresponding official guides linked in `README.md`.
