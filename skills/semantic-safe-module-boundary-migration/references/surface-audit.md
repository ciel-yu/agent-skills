# Semantic Surface Audit

Map the surface from the caller's point of view.

Audit in this order:

1. **Discovery** - how callers find the module: import path, package export, registry key, DI token, config name, command name.
2. **Entry points** - exported symbols, constructors, functions, classes, handlers, factories.
3. **Invocation contract** - parameters, defaults, generics, overloads, sync vs async, initialization order.
4. **Result contract** - return values, emitted events, mutations, persisted data, thrown errors, nullability.
5. **Behavioral invariants** - ordering, idempotency, retries, caching, transactions, cleanup, resource ownership.
6. **Caller assumptions** - timing, side effects, singleton behavior, environment requirements, partial-failure semantics.

Classify each surface element as:

- **Required** - must remain stable for migration safety.
- **Relied-on incidental** - not intended, but current callers depend on it.
- **Ambiguous** - evidence conflicts or is too thin.
- **Dead** - exported or documented but unused in the migration scope.

Do not define the surface from exports alone; confirm it against real consumers.
