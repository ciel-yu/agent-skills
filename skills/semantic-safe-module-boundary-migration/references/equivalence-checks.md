# Surface Equivalence Checks

Verification should answer: "Would an existing caller observe a meaningful change?"

Check the surface in layers:

1. **Discovery equivalence** - old and intended import/discovery paths resolve as planned.
2. **Shape equivalence** - exported names, signatures, types, and schemas match the preserved contract.
3. **Behavior equivalence** - same happy-path results for representative callers.
4. **Failure equivalence** - same error class, status, nullability, retryability, or failure semantics where preserved.
5. **Side-effect equivalence** - same writes, events, ordering, caching, transactions, cleanup, or singleton behavior when preserved.

Evidence sources, strongest first:

- existing tests that already pin the surface
- added migration tests at important boundaries
- type or schema checks
- minimal driver scripts that import and execute representative flows
- runtime smoke checks through the real surface

Inspection alone is not equivalence evidence.
