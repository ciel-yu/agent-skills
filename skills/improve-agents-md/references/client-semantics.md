# `AGENTS.md` Client Semantics

Verify live documentation and, when possible, observe the active instruction chain. Client behavior changes independently of the `AGENTS.md` format.

## Open format

[agents.md](https://agents.md/) defines `AGENTS.md` as plain Markdown with no required fields or headings. Its ecosystem guidance says nearer files take precedence, but each client owns traversal, timing, merging, and limits. Treat the site as a portability convention, not an executable loader specification.

## Codex

Current Codex documentation describes this chain:

1. Global scope: read `AGENTS.override.md` from `CODEX_HOME` when present; otherwise read `AGENTS.md`.
2. Project scope: start at the project root and walk down to the current working directory.
3. Per directory: use at most one non-empty file, checking `AGENTS.override.md`, then `AGENTS.md`, then configured fallback names.
4. Merge: concatenate from root toward the current directory; later, nearer guidance has higher precedence.
5. Limit: stop when combined project instructions reach `project_doc_max_bytes` (32 KiB by default).
6. Timing: build the chain once per run or TUI session.

Consequences:

- A nested file below the session's current working directory is not part of the documented startup chain.
- Starting Codex at the repository root and merely editing a deeper file is not equivalent to starting it with `--cd` in that subtree.
- `AGENTS.override.md` replaces the regular file at its directory; it is not an additive suffix.
- Fallback filenames work only when configured.

Verify with a non-mutating prompt from both root and relevant subdirectories:

```bash
codex --ask-for-approval never "List the instruction sources you loaded and summarize their precedence."
codex --cd path/to/subdir --ask-for-approval never "List the instruction sources you loaded and summarize their precedence."
```

Source: [OpenAI, Custom instructions with AGENTS.md](https://learn.chatgpt.com/docs/agent-configuration/agents-md).

## GitHub Copilot agents

GitHub documents these behaviors for agent instructions:

- one or more `AGENTS.md` files may live anywhere in a repository
- the nearest `AGENTS.md` in the directory tree takes precedence
- repository-wide and path-specific Copilot instruction files may also apply to the same request
- conflicting instruction sets should be avoided

Feature support differs among Copilot cloud agent, code review, CLI, IDE agent mode, and web chat. Confirm the target surface. When the UI exposes response references, verify that the intended instruction file appears there.

Source: [GitHub, Adding repository custom instructions](https://docs.github.com/en/copilot/how-tos/copilot-on-github/customize-copilot/add-custom-instructions/add-repository-instructions).

## Configurable consumers

Some clients can be configured to treat `AGENTS.md` as a context filename even when it is not their default. Configuration is part of the loading contract; do not assume another developer or CI agent shares it. Record the required setting and verify it in the target environment.

For example, Gemini CLI documents `context.fileName` and hierarchical plus just-in-time context loading. Only apply those semantics after confirming that `AGENTS.md` is configured as a context filename.

Source: [Gemini CLI, Provide context with GEMINI.md files](https://geminicli.com/docs/cli/gemini-md/).

## Unknown or multiple clients

1. List every required consumer.
2. Build a small matrix: filename, traversal, merge or replacement, timing, limit, verification method.
3. Use the simplest layout whose behavior is documented for all required consumers.
4. If semantics conflict, keep one root `AGENTS.md` and move client-specific behavior into that client's configuration rather than pretending nested files are portable.
5. Report any unverified consumer; do not invent a shared precedence rule.