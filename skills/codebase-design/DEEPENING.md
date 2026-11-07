# Deepening a module

Gather one body of domain knowledge behind a useful interface. Similar syntax alone does not
justify merging code with different reasons to change.

1. Identify the shared policy, invariants, state owners and direct consumers.
2. Sketch the caller's operation and what coordination the new interface hides.
3. Trace dependency lifetimes, configuration, acquisition, cancellation and partial-failure cleanup.
4. Keep domain knowledge local. Introduce ports or adapters for real substitution, platform,
   distribution or trust requirements; follow repository DI conventions. Adapter count is not proof
   that a boundary is useful. Do not invent providers merely to make tests easy to mock.
5. Migrate callers, exports, docs and obsolete paths together according to compatibility policy.
6. Verify supported behavior through the resulting interface using [TDD](../tdd/SKILL.md) or the
   [structural-change workflow](../structural-code-change/SKILL.md), as appropriate.

Preserve regression coverage while restructuring. Retire superseded tests only after retained
coverage protects their meaningful behavior; lower-level tests may still protect distinct contracts.
Keep internal implementation choices private without hiding legitimate observable effects.
