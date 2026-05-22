# Ambiguity Gate

Open this gate when the pattern document is not specific enough for deterministic review.

## Trigger conditions

Stop and ask when any of these are true:

- core rules are mostly aspirational prose (`clean`, `scalable`, `well-structured`) with no operational meaning
- multiple sections conflict about boundaries, ownership, or layering
- the document describes goals but not reviewable constraints
- the requested review surface is unclear (`repo` vs `diff` vs `selected files`)
- the document implies important rules that would materially change the score, but they are not stated clearly enough

## What to ask

Ask only the minimum clarifying questions needed to unblock deterministic review. Prefer questions such as:

- Which review surface should control the score: repo, diff, or selected files?
- Which of these implied rules is actually mandatory?
- Should this section be treated as architecture guidance or optional style preference?
- When the codebase already has an older pattern, should the score use the new document or current practice as the source of truth?

## Hard rule

Do **not** assign a final score while the Ambiguity Gate is open.

You may return:

- extracted rules that are clear
- ambiguous rules that block grading
- the exact questions required to continue

But do not present a completed review.
