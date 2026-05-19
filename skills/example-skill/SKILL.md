---
name: example-skill
description: A minimal, specification-aligned example Agent Skill template.
---

# Example Skill

## Purpose

Use this skill as a starting template when creating new skills in this repository.

## When to use

- You need a new skill scaffold under `skills/<skill_name>/`.
- You want a consistent metadata and instruction format.

## Inputs

- `skill_name`: target skill directory name (kebab-case).
- `goal`: what the skill should help the agent accomplish.

## Steps

1. Create `skills/<skill_name>/`.
2. Add a `SKILL.md` with front matter (`name`, `description`).
3. Document purpose, usage conditions, inputs, and execution steps.
4. Keep any helper resources inside the same skill directory.

## Expected output

A new skill directory that follows the repository's Agent Skill layout conventions.
