# Evaluation

Separate three questions:

1. Is the bundle valid and safe?
2. Does the description activate on the right requests?
3. Does the skill improve observable task outcomes enough to justify its cost?

Static review answers only the first question.

## Trigger evaluation

Build realistic should-trigger and should-not-trigger queries. Near misses are more informative than obviously unrelated prompts.

Include variation in:

- formal and casual wording
- explicit and implicit domain references
- short and context-heavy requests
- file paths, input formats, and user constraints
- adjacent tasks that share keywords but need a different capability

For a substantial description change, aim for roughly 8–10 positive and 8–10 negative queries. Split them into a revision set and a held-out validation set; do not tune against the held-out prompts.

Run each query through the actual target client and observe whether it loads the skill. Because routing is nondeterministic, repeat each query when practical and record the activation rate. Compare the old and new descriptions on the same queries, client, model, and settings.

Do not infer activation from semantic similarity alone. If the client cannot be run, label the description review **static-only**.

## Output evaluation

Start with 2–3 representative tasks plus one edge case. Each case needs:

- a realistic prompt
- input files when needed
- an expected observable result
- assertions that distinguish useful execution from plausible prose

Run paired conditions in clean contexts:

- revised skill versus the previous skill, or
- skill versus no skill when establishing whether the skill adds value

Keep model, client, permissions, environment, prompt, and input data constant. Capture outputs, assertion evidence, tool calls, duration, and token counts when available.

Prefer deterministic checks for file validity, schemas, counts, commands, and state transitions. Use human or blinded model review only for qualities that cannot be checked mechanically.

## Assertions

Good assertions cover behavior:

- output parses or executes
- required invariants and boundary cases hold
- the documented fast path is used for a routine task
- a fragile operation validates before mutating state
- failure output enables recovery
- an unrelated task is not pulled into the skill

Weak assertions merely search for headings, exact prose, or self-reported compliance.

## Interpreting results

- A valid manifest is not proof that the skill helps.
- More activation is not better if false positives rise.
- A larger bundle is not better if quality is unchanged and cost rises.
- One client or model result does not establish portability.
- If the revised skill performs worse, revert or narrow the responsible change rather than adding more instructions blindly.

Current evidence supports paired evaluation. [SkillsBench v4](https://arxiv.org/html/2602.12670) reports substantial average gains for curated skills but large differences by harness and task; compact and standard-length skills outperform comprehensive prose. Treat efficacy as an empirical property of a skill, task distribution, model, and client—not a formatting property.

## Bundled regressions

`evals/evals.json` exercises:

- a valid skill that declares no design pattern and needs no companion directories
- a focused description revision backed by positive and near-miss queries
- a pre-publish security review that must not execute untrusted remote setup

Run fixture-editing cases in copied workspaces. Preserve the canonical fixtures.