# Product Spec Extraction Workflow

## 1. Find authoritative product surfaces

Start with the most user-visible evidence:

- README, docs, demo scripts, screenshots
- app shell, navigation, landing pages, dashboards
- route maps, screen registries, command menus
- API surface or CLI commands that define the product contract

Prefer shipped entry points over deep implementation files.

## 2. Build an evidence ledger

For each strong signal, capture:

- source path
- observed behavior
- likely product meaning
- confidence level

Do not collapse evidence and inference into one note.

## 3. Reconstruct core product shape

Extract:

- primary user or actor
- main jobs to be done
- core entities
- major features
- external integrations
- permissions or plan splits

If the repo supports several actors, state each clearly instead of forcing one fake persona.

## 4. Reconstruct user flows

Trace at least one complete flow from entry point to task completion using:

- route transitions
- button / form actions
- handler or API calls
- resulting UI states or persisted records

Prefer flows that are both reachable and central to the product.

## 5. Separate observed facts from inferred intent

- **Observed**: directly supported by code, copy, tests, or docs
- **Inferred**: likely product meaning supported by multiple clues
- **Unknown**: missing business rationale, prioritization, or audience assumptions

When in doubt, downgrade certainty.

## 6. Write the current-state spec

Generate the document from the template.

Requirements:

- cite representative evidence paths inline
- mark confidence where it matters
- include only meaningful shipped features
- capture unresolved intent in `Open Questions`

## 7. Validate before handing off

Check that a new reader can answer:

- What does this product do?
- Who uses it?
- What are the main flows?
- Which features are definitely shipped?
- What is still ambiguous from code alone?
