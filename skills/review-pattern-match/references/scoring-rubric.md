# Scoring Rubric

Use a total score of `0-100` plus four dimension scores.

## Dimensions

- **Architecture** - boundaries, dependency direction, layering, ownership
- **Code Organization** - file placement, module responsibility, decomposition, surface consistency
- **Naming & API Design** - names, interface shape, abstraction level, public contract clarity
- **Idiomatic Implementation** - repeated implementation patterns the document requires, not generic style

Default weights:

- `Architecture`: 35
- `Code Organization`: 25
- `Naming & API Design`: 20
- `Idiomatic Implementation`: 20

If the document clearly emphasizes one dimension more heavily, say so and adjust the weighting explicitly before scoring.

## Scoring bands

- **90-100** - strong match; only isolated or low-impact deviations
- **75-89** - generally aligned; some meaningful drift, but the core pattern is intact
- **50-74** - mixed match; repeated deviations weaken the documented pattern
- **25-49** - weak match; the code follows a substantially different approach
- **0-24** - near-total mismatch or insufficient compliant evidence

## Deduction model

Deduct based on rule importance and repetition.

- **Major documented rule violated repeatedly**: `-10` to `-20`
- **Major documented rule violated once**: `-6` to `-10`
- **Secondary rule violated repeatedly**: `-4` to `-8`
- **Secondary rule violated once**: `-2` to `-4`
- **Ambiguity requiring inferred judgment**: cap the affected dimension at `79` unless the user clarifies

Do not deduct for:

- preferences not stated in the pattern document
- formatting or style issues unless the pattern document treats them as structural
- areas outside the reviewed surface

## Output contract

Return:

- total score
- one line per dimension with short justification
- score confidence: `High`, `Medium`, or `Low`
- note any weighting changes

## Confidence rules

- **High** - most major rules are exact and the reviewed surface is representative
- **Medium** - some conclusions are section-level or the surface is partial
- **Low** - multiple important rules are inferred or the document stayed partially ambiguous
