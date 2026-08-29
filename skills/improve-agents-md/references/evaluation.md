# Evaluation

Static correctness is mandatory. Behavioral evaluation is required before claiming that a new or substantially rewritten `AGENTS.md` improves agent performance.

## Static checks

1. Confirm the target client's live loading documentation.
2. Reveal the active instruction chain from root and each intended working directory.
3. Trace every command, path, and policy to repository or user evidence.
4. Check parent, child, override, and other instruction files for conflicts.
5. Run safe canonical commands when the task authorizes execution; otherwise verify them against executable configuration and state the limitation.
6. Check links and identify whether they are optional references or automatically loaded inputs.
7. Scan for secrets, destructive commands, and claims of enforcement that Markdown cannot provide.

## Paired behavior protocol

Use the previous file—or no file—as the baseline.

1. Select 2–3 realistic tasks where the candidate instructions should matter.
2. Add one unrelated task to detect interference from always-loaded guidance.
3. Run baseline and candidate with the same client, model, repository state, prompt, permissions, and time limit.
4. Start each run with a clean context. Repeat nondeterministic cases when practical.
5. Grade observable outcomes rather than whether the agent paraphrases the file.

Useful assertions:

- selects the canonical package manager or build command
- performs required setup before running a command
- avoids an immutable or generated path
- chooses a targeted test instead of a known expensive full suite
- follows the active rather than legacy implementation path
- loads the intended nested file in the documented workflow
- completes unrelated work without extra mandatory steps

Record pass/fail evidence, tool calls, wall-clock time, and tokens when exposed by the client.

## Interpret results conservatively

- Keep policy-backed safety constraints even when a small task sample does not exercise them.
- Remove or narrow instructions that add work without changing the intended outcome.
- Do not generalize a result across clients or models that were not tested.
- Do not claim correctness, cost, or latency improvement from a static review alone.

Recent evidence is mixed: developer-written context can help specific process decisions, while generic or generated context often adds cost without improving correctness. See [Evaluating AGENTS.md](https://arxiv.org/html/2602.11988v1), [On the Impact of AGENTS.md](https://arxiv.org/html/2601.20404v2), and [Do Context Files Help Coding Agents?](https://arxiv.org/html/2607.27250v1).

## Bundled cases

`evals/evals.json` contains lightweight regression cases for:

- removing unsupported and inferable root guidance
- returning no findings for an already minimal file
- handling Codex nested-file semantics when sessions always start at root

Run them in copied workspaces; never mutate the canonical fixtures.