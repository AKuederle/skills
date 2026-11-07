# Deepening

Consolidate one body of domain knowledge, not coincidentally similar syntax. Identify policy,
invariants, owners, consumers, and the caller operation. Hide coordination; trace configuration,
lifetimes, acquisition, cancellation, and partial-failure cleanup.

Use ports/adapters for real substitution, platform, distribution, or trust needs under repository DI
conventions, not adapter counts or mock convenience. Migrate consumers, exports, docs, and obsolete
paths under compatibility policy. Verify via [TDD](../tdd/SKILL.md) or [structural
checks](../structural-code-change/SKILL.md). Preserve coverage during restructuring; retire
superseded tests only once retained tests cover their meaningful behavior. Keep distinct lower-level
contracts and legitimate observable effects.
