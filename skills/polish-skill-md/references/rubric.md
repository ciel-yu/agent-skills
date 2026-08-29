# Review Rubric

Apply authoritative requirements before preferences. A valid skill need not follow a named design pattern or a fixed section template.

## Authority order

1. explicit user and repository requirements
2. current [Agent Skills specification](https://agentskills.io/specification) and target-client documentation
3. observed trigger and task evaluations
4. official authoring guidance
5. optional design lenses and style preferences

When authorities differ, satisfy the target clients or report the incompatibility. Never present a recommendation as a specification rule.

## Severity

### Critical

- credential exposure, unauthorized access, destructive behavior, or data exfiltration risk
- remote content or dependencies executed without an appropriate trust and integrity check
- bundled instructions that enable prompt injection or bypass the user's stated controls
- a rewrite that silently weakens a safety invariant

### High

- invalid required frontmatter or a `name` that does not match its directory
- description scope that predictably misses intended tasks or captures materially different tasks
- missing, broken, stale, or unusable resources required by the workflow
- scripts that block for interaction, hide failures, or require undocumented dependencies
- instructions incompatible with the target client or execution environment

### Medium

- generic knowledge that adds context without adding capability
- an exhaustive or heavyweight workflow without applicability boundaries or a fast fallback
- copied documentation, deep reference chains, duplicated instructions, or eager resource loading
- optional frontmatter that is inaccurate or too vague to support execution
- an improvement claim unsupported by a relevant evaluation

### Low

- inconsistent terms, weak ordering, unnecessary prose, stale examples, or formatting that slows scanning

If no item fails, return **No findings** and recommend no rewrite.

## Specification checks

### Required frontmatter

`name`:

- 1–64 characters
- lowercase letters, numbers, and hyphens only
- no leading, trailing, or consecutive hyphens
- exactly matches the parent directory name

`description`:

- 1–1024 characters
- says what the skill does and when it should be used
- contains terms a client can match to realistic user intent

The open specification does not require imperative phrasing or negative scope. Prefer a concise third-person capability statement plus an explicit `Use when...` clause when that form works across target clients. Add negative scope when near-miss evaluations show ambiguity.

### Optional frontmatter

- `license`: short identifier or bundled license reference
- `compatibility`: no more than 500 characters; state real environment requirements only
- `metadata`: string-to-string mapping
- `allowed-tools`: experimental and client-dependent; verify before relying on it

Check target-client additions separately. For example, a client may reject reserved words or XML tags that the open specification does not discuss.

### Body and resources

The specification imposes no mandatory body headings or pattern taxonomy. It recommends:

- focused procedural instructions, examples, and edge cases
- `SKILL.md` below roughly 500 lines / 5,000 tokens
- conditional detail in separate resources
- direct, relative references from `SKILL.md`; avoid deep reference chains

Treat these as performance guidance, not hard schema failures.

## Description review

Clients advertise at least the skill name and description; the description is the primary routing text, not the only metadata visible.

Check that it:

- covers the coherent task class the body actually supports
- names concrete capabilities and input or domain cues
- avoids internal implementation details that do not aid selection
- avoids unrelated keywords added only to increase activation
- distinguishes adjacent tasks when confusion is plausible
- has trigger evidence before being called improved

## Instruction review

Keep content that provides something the base agent is likely to lack:

- a proven sequence, calibrated default, or exact command
- domain invariants, parsing quirks, format constraints, or failure recovery
- explicit applicability and termination conditions
- validation tied to observable output

Tighten, split, or delete:

- generic explanations and motivational prose
- menus with no default or decision rule
- repeated warnings and copied upstream documentation
- task-instance answers disguised as reusable expertise
- rigid steps whose cost exceeds the routine case

Match specificity to fragility. Flexible work benefits from goals and heuristics; brittle operations need exact steps, validation, and recovery.

## Progressive disclosure

- Keep instructions needed on nearly every invocation in `SKILL.md`.
- Move conditional depth to focused resource files.
- Link every required resource from `SKILL.md` with a concrete when-to-load cue.
- Do not split content merely to satisfy a line count; each extra read has execution cost.
- Put long reusable templates in `assets/` and repeated deterministic logic in `scripts/` when doing so improves execution. Directory choice is not a taxonomy requirement.

## Scripts and operability

For each script or executable command:

- document prerequisites or declare them in `compatibility`
- pin dependencies when reproducibility matters
- accept flags, environment variables, or stdin instead of interactive prompts
- provide concise `--help`, actionable errors, and stable outputs
- use structured output when another step consumes it
- test the normal path and plausible failure paths
- avoid fetching and executing mutable remote content

## Security review

Inspect every file during a full or pre-publish review, including images and apparently passive resources.

Look for:

- network requests, telemetry, uploads, webhooks, and external imports
- shell pipelines, encoded payloads, dynamic execution, and privilege changes
- reads of credentials, home directories, browser data, SSH material, or unrelated files
- instructions imported from mutable external sources
- tool permissions broader than the stated capability
- dependencies without a version, provenance, or integrity strategy

Do not execute suspicious setup to confirm it is suspicious. Report the evidence and a safer design.

## Revision decisions

Use four labels:

- **Keep** — valid and load-bearing.
- **Tighten** — same behavior with less or clearer context.
- **Split** — conditionally needed material belongs in a linked resource.
- **Delete** — unsupported, unsafe, obsolete, duplicated, or demonstrably harmful.

Never delete or create files solely because an optional design lens suggests a different directory shape.