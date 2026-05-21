# Inference Signal Catalog

Use this catalog to turn code evidence into product-level statements without over-claiming.

---

## Evidence Type → Product Meaning

| Evidence in repo | Usually supports |
|---|---|
| App title, nav labels, page headings, CTA copy | Product positioning, information architecture, primary actions |
| Route files, screen components, command handlers | User-visible surfaces, major workflows |
| Forms, input schemas, validation rules | User inputs, required fields, system expectations |
| API endpoints, service methods, background jobs | Product operations, automation, integrations |
| Data models, enums, status fields | Core entities, lifecycle stages, business objects |
| Permission checks, role guards, billing gates | User roles, plan tiers, admin vs end-user boundaries |
| Tests, fixtures, seed data | Happy paths, critical scenarios, supported states |
| Docs, screenshots, Storybook, demo scripts | Intended usage, feature naming, UX framing |

---

## Confidence Heuristics

### High confidence

- Same behavior appears in UI copy, route/action wiring, and tests
- Feature is reachable from a primary navigation path
- Multiple files agree on the same entity or workflow name

### Medium confidence

- Behavior is wired end-to-end but lightly surfaced in copy
- Feature exists in code and UI, but no tests or sparse naming clarity

### Low confidence

- Behavior exists only in helpers, flags, TODOs, or partial screens
- Intent is guessed from implementation details rather than user-facing evidence

Low-confidence claims belong in `Open Questions` unless they are needed to explain a visible surface.

---

## AI / Automation Signals

Use these only when backed by code or visible product copy.

| Signal in code | Likely product capability |
|---|---|
| `openai`, `anthropic`, `gemini`, `llama`, prompt templates | Text generation, summarization, extraction, chat, classification |
| Embedding or vector libraries | Semantic search, retrieval, recommendations, clustering |
| OCR libraries, vision APIs, image upload + analysis flows | OCR, image recognition, document extraction |
| TTS / STT SDKs, microphone UI, waveform components | Voice input, transcription, speech output |
| Queue workers or scheduled jobs tied to model calls | Background AI processing or automation |
| Recommendation ranking logic, personalization features | Recommendations, smart suggestions |

If the repo contains an AI SDK but no product-visible usage, note the integration under technical signals, not as a shipped feature.

---

## Common Misreads to Avoid

- Internal model names are not user-facing features by themselves
- CRUD endpoints do not automatically imply a meaningful product workflow
- Admin tools are not the main product unless the repo proves they are
- Feature flags, experiments, and dead code are not shipped requirements unless reachable
- Database schema alone does not prove the UX or business intent
