# Review Workflow

## 1. Identify the review surface

Classify the request as one of:

- **`repo`** - the whole repository or a meaningful subsystem
- **`diff`** - a PR, patch, commit range, or staged change
- **`selected files`** - a user-provided file set

Name the surface explicitly in the output. Do not pretend a partial review was repo-wide.

## 2. Read the pattern document first

Extract:

- explicit required structures
- forbidden structures or practices
- naming and API-shape rules
- organization and dependency rules
- idiomatic implementation preferences that are stated as requirements, not taste

Convert prose into reviewable rules. Good rule: "controllers must not call the database directly." Bad rule: "keep the code clean and scalable."

## 3. Normalize the rules

For each rule, record:

- **Rule ID** - short stable label
- **Pattern source** - exact quote, section heading, or inferred note
- **Dimension** - `Architecture`, `Code Organization`, `Naming & API Design`, or `Idiomatic Implementation`
- **Determinism** - `deterministic` or `needs clarification`

Open the Ambiguity Gate immediately if too many important rules land in `needs clarification`.

## 4. Collect code evidence

Review only enough code to judge the extracted rules:

- for **`repo`**: inspect top-level structure, primary entrypoints, main modules, and representative implementations
- for **`diff`**: inspect changed files first, then read adjacent owners/callers only when needed to judge the rule
- for **`selected files`**: stay within the supplied files unless a dependency is necessary to verify the rule

Prefer direct evidence:

- module boundaries
- imports and dependency direction
- public interfaces
- filenames and folder structure
- repeated implementation patterns
- tests only when they materially prove the pattern

## 5. Write findings

Findings first. Order by severity:

1. direct contradiction of a core documented rule
2. architecture or dependency violations
3. code-organization mismatches
4. naming or API-shape mismatches
5. idiomatic-implementation mismatches
6. residual uncertainty or minor drift

Each finding should contain:

- severity
- short title
- code reference(s)
- pattern linkage
- evidence-strength tag
- concise impact
- concise remediation

## 6. Score the match

Use [scoring-rubric.md](./scoring-rubric.md).

Score after findings, not before. Let the score summarize the evidence; do not let the score replace it.

## 7. Close with coverage and open questions

Always state:

- what was reviewed
- what was not reviewed
- which conclusions are exact vs inferred
- only the questions that would change the result
