# Evidence Sufficiency Criteria

Before generating the inferred Product Spec, every condition below must be satisfied.

---

## Must Be Met

- [ ] **Current-state product summary is supportable**: one plain-language sentence can describe what the product appears to do
- [ ] **Core surfaces are identified**: main pages, routes, commands, or API entry points are known
- [ ] **Meaningful features are inventoried**: the document covers the major shipped capabilities, not just technical modules
- [ ] **At least one end-to-end flow is reconstructed**: a user can be traced from entry point to a completed outcome
- [ ] **Primary entities or actors are named**: the spec can describe who acts and what objects move through the system
- [ ] **Integrations and AI signals are grounded**: external services or AI capabilities are included only when evidence supports them
- [ ] **Unknown intent is captured explicitly**: unresolved goals, audiences, or prioritization are moved to open questions

---

## Judgment Principles

- If behavior is visible but business intent is missing, generate the spec and mark the missing intent as `Unknown`
- If even the shipped behavior is too unclear, keep auditing source or ask for adjacent artifacts such as docs, screenshots, or production URLs
- Ask one more evidence-gathering round rather than guessing at product strategy
- When evidence is sufficient for current-state understanding, move forward without waiting for perfect certainty
