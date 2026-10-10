# Evidence and Value Method

Use this method before drafting, reviewing, or moving any `AGENTS.md` instruction.

## 1. Identify the consumer

Record the target client and workflow:

- client and version, if known
- where the session starts
- recognized filenames
- traversal direction and stopping point
- merge, replacement, and precedence behavior
- load timing: startup, per file, or on demand
- size or byte limits
- a command or UI that reveals active instruction sources

`AGENTS.md` standardizes a filename and plain-Markdown convention. It does not guarantee identical loading behavior. If the consumer is unknown, keep one root file and do not introduce layers.

## 2. Build an evidence ledger

Use sources in this order:

1. explicit user or organization policy
2. executable configuration, package manifests, scripts, CI, hooks, and permissions
3. current code and tests that demonstrate the active convention
4. maintained project documentation
5. version history only when needed to distinguish current from legacy behavior

An existing `AGENTS.md` line is a claim to verify, not independent proof. Record the source path or user statement behind every command, path, architecture fact, and policy.

## 3. Apply the line-value gate

A retained or proposed instruction must answer all applicable questions:

| Question | Pass condition |
|---|---|
| Supported? | A current repository or user source backs it. |
| Non-inferable? | The client is unlikely to derive it cheaply and reliably from nearby files. |
| Behavior-changing? | It prevents a plausible error, avoids measurable waste, or carries explicit policy. |
| Correctly scoped? | It applies throughout the directory scope where the client loads it. |
| Durable? | Normal daily work will not make it stale. |
| Properly enforced? | Guidance stays in Markdown; mandatory controls use an enforcement mechanism. |

Classify each item:

- **Keep** — passes the gate as written.
- **Tighten** — valuable, but broader or longer than necessary.
- **Relocate** — valuable only at another scope or in optional documentation.
- **Delete** — unsupported, stale, duplicated, inferable, or behaviorless.
- **Omit** — candidate content that never belonged in the file.

Explicit safety or compliance policy does not need a prior incident to have value, but it still needs an authoritative source and correct scope.

Delete a sentence that adds no behavioral value instead of polishing its wording. Whether a rule is redundant depends on the target agent's default behavior, not the reviewer's familiarity; use paired runs to settle consequential disputes.

## 4. Prioritize scarce information

High-value candidates:

- the one canonical bootstrap, build, test, or generation command
- required setup that is not encoded by the command itself
- warnings that a full suite is unusually slow, destructive, or environment-dependent
- generated, immutable, vendored, append-only, or externally managed paths
- non-obvious contracts shared with another system
- canonical-versus-legacy decisions where the repository contains both patterns
- local exceptions that genuinely differ from parent scope

Usually omit or relocate:

- a README summary or directory tree the client can inspect cheaply
- generic language, framework, testing, or code-quality advice
- formatter and linter rules already enforced by tooling
- every variant of a command instead of the safe default
- copied architecture or API documentation
- sprint state, current progress, ownership notes, and temporary workarounds
- instructions for a rare task that can live in a runbook or skill

A short repository map is justified only when the layout is misleading, canonical entrypoints are otherwise hard to discover, or maintained documentation is absent.

## 5. Choose scope from semantics

- **Root:** only rules that apply repository-wide.
- **Nested:** only local additions or overrides, and only when the target client loads them in the intended workflow.
- **Companion document:** optional detail the agent may read when a named condition occurs.
- **Tooling or policy system:** constraints that must be enforced rather than suggested.

Do not duplicate parent rules in children. State an override explicitly when both layers accumulate. Do not create a child file whose only content points back to its parent.

A Markdown link is a route to optional context, not proof that the client loads or obeys the target file.

Write each pointer as a reading decision: **when [distinct task condition], read [maintained source] for [needed guidance]**. Include one cue per distinct task branch rather than several synonyms for one case. For example: “Before changing the database schema, read `docs/migrations.md` for compatibility checks.” If needed detail is missed, improve the cue first; inline it only when optional loading remains unreliable or the constraint must always be visible.

Choose disclosure by applicability, not length alone: inline what every affected task needs and link detail used only by some branches. Even nonduplicated rules can overwhelm the root. Each new document should earn its extra read and the maintainer's burden of finding and updating it.

## 6. Separate guidance from controls

Use `AGENTS.md` to explain the safe action and why it matters. Use the repository's actual control plane for guarantees:

- formatter, linter, type checker, or test for machine-checkable rules
- hook or permission rule for blocked tools and paths
- CI or branch protection for merge requirements
- sandboxing and credentials policy for environmental boundaries

If enforcement is required but absent, keep the warning when it prevents harm and report the missing control. Never claim that Markdown alone enforces it.

## 7. Draft without a mandatory template

Order the surviving content for early visibility:

1. critical scope or safety constraints
2. canonical commands and prerequisites
3. non-obvious conventions and decision rules
4. conditional pointers to maintained detail

Use headings only when they improve scanning. Keep a decision's rule, reason, exceptions, and safe alternative together; give each meaning one authoritative home. A short ordered procedure and a flat reference list are both valid—organize for execution rather than forcing one shape.

State the desired action concretely. Keep necessary prohibitions backed by user or repository policy, with a safe alternative where available; brevity is not grounds to weaken a boundary.

For retained procedures, make completion both checkable and exhaustive within the affected scope. For example, “regenerate every changed schema's client with the canonical command and verify a clean generation diff” is a usable endpoint only when repository evidence supports those checks. Avoid fuzzy endpoints such as “understand the architecture” and avoid adding blanket workflow obligations.

A valid result may be a few bullets, one root file, no nested files, no change, or no `AGENTS.md` at all.