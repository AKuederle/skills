---
name: tdd
description: Test-driven development for supported behavior changes and bug fixes. Use when the user asks for test-first work, red-green-refactor, or integration tests. Do not use for mechanical changes that static tooling and existing tests already verify.
---

# Test-driven development

Make supported behavior executable through a meaningful interface. Use the repository's domain
vocabulary and respect relevant ADRs.

## Test quality

- Prefer high-level behavioral and integration tests over highly mocked lower-level tests.
- Keep tests structure-insensitive, deterministic, and isolated.
- Use independent test oracles.
- DAMP over DRY.
- Name the regression a new test would detect before deciding to write it.
- Focus each test on one behavior; use multiple assertions when they establish that behavior.
- Keep the behavior under test real. Use test doubles where they improve control without
  replacing the behavior being verified.

See [tests.md](tests.md) for examples when choosing observations, test doubles, or expected values.

## Choose the interface

Test through supported interfaces within the agreed scope. State what the chosen interface catches
and misses. Discuss consequential testing or scope decisions; routine tests need no separate approval.
Consult `codebase-design` when the interface itself needs redesign.

The contract determines observable behavior. Return values, errors, persisted state and boundary
effects can all matter. Call counts, ordering and database reads are valid observations when the
contract concerns those effects, rather than incidental internal collaboration.

## Red–green–refactor

Work in vertical slices:

1. **Red:** write one focused behavior test and confirm it fails for the intended reason.
2. **Green:** implement enough to satisfy it while preserving existing contracts; run affected tests.
3. **Refactor:** improve the design under green, without changing behavior. Repeat.

Control time, randomness, readiness and shared state where they affect repeatability. Derive
expectations independently of the implementation under test, using the contract, worked examples,
trusted fixtures or an independent model. A separate calculation can be valid; duplicating the
implementation's algorithm risks reproducing its mistake.

## Avoid low-value coverage

- **Implementation coupling:** freezes private structure or incidental internal call sequences.
- **Tautological assertions:** reuse the code under test to produce the expected result.
- **Compiler-redundant checks:** repeat facts already enforced by declarations. Type tests can
  protect non-trivial inference, overload selection, generic correlation or capability boundaries.
- **Change detectors:** freeze source text, constants or export inventories without proving behavior.
- **Horizontal slicing:** write a batch of speculative tests before learning from implementation.

For mechanical edits, use static checks and relevant existing tests. Do not manufacture a red–green
cycle or permanent coverage merely to record the diff. Before adding a test, identify a plausible
bug it catches that the compiler and existing checks do not.
