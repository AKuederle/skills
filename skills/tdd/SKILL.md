---
name: tdd
description: Test supported behavior changes and bugs through behavioral increments. Use for test-first work and integration tests, not mechanical edits covered by tooling/existing tests.
---

# TDD

Use supported interfaces, domain vocabulary, and relevant ADRs. Explain what the interface
catches/misses; discuss consequential scope/testing choices, not routine seam approvals. Consult
[codebase-design](../codebase-design/SKILL.md) for redesign.

- Prefer high-level behavioral/integration tests to highly mocked units.
- Structure-insensitive, deterministic, isolated tests; independent oracles; DAMP over DRY.
- Name the regression before deciding to write a test. One behavior can need multiple assertions.
- Keep tested behavior real; doubles provide control without replacing it. The contract determines
  observable results, errors, persistence, and effects, including call counts/order when promised.

Work in caller-visible behavioral increments, preferably driven by a meaningful integration test,
not implementation-sized steps. For bug fixes, prove failure for the intended reason before the fix
and success after. Temporary diagnostic probes are fine. Preserve coverage during refactoring;
afterward rewrite implementation-coupled tests and retire superseded ones while preserving distinct
behavior. Tests serve ongoing regression value, not development history.

Control time/randomness/readiness/shared state. Derive expectations from contracts, worked examples,
trusted fixtures, or independent models, not the tested code. Duplicating its algorithm is weaker,
not always incapable of failure.

Reject private-structure/change-detector tests, tautological expectations, speculative horizontal
batches, and compiler-redundant checks. Keep meaningful type tests for inference, overloads, generic
correlation, or capability boundaries. For mechanical work use static/existing checks; no
manufactured red-green or permanent diff tests. Read [tests.md](tests.md) for observations, doubles,
and oracle examples.
