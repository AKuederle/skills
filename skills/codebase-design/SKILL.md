---
name: codebase-design
description: Design deep modules, coherent interfaces, and explicit ownership for new behavior, interface/dependency changes, restructuring, and design review. Companion skills own implementation workflow and testing.
---

# Codebase design

Make requested behavior easy to use correctly, reason about locally, and change safely. Future
engineers and agents should need less context and find one owner per responsibility.

- Interface-first design: deep modules and information hiding.
- Locality of reasoning and change; explicit states, transitions, invariants, and ownership.
- Avoid implicit state machines and temporal coupling. Derive rather than synchronize duplicated
  mutable state.
- Design by contract: assume competent callers; reject defensive programming for unsupported misuse.
  Preserve supported invariants and explicit guarantees. Validate trust boundaries and protect
  against corruption or destruction of unrelated data. Review feedback does not expand scope.
- Fix common causes; prefer subtraction over speculative generality.

An interface includes types, invariants, ordering, errors, and configuration. Depth hides caller
coordination, not merely code. Use domain vocabulary.

To deepen a module, consolidate shared policy, not similar syntax. Trace configuration, ownership,
acquisition, cancellation, and partial-failure cleanup. Use ports/adapters for real substitution,
platform, distribution, or trust needs under repository DI conventions, not mock convenience.
Migrate consumers, exports, docs, and obsolete paths under the compatibility policy.

Tips: temporary skeleton views; authorized focused subagent summaries; refreshed dependency graphs;
start at highest leverage; deletion test: does complexity disappear or return to callers?

For consequential interface choices, sketch two materially different interfaces with realistic
callers. Compare caller coordination, valid states, lifecycle ownership, and tradeoffs; recommend
the smallest sufficient design.

[implement-code-change](../implement-code-change/SKILL.md) owns delivery;
[TDD](../tdd/SKILL.md) owns behavior verification/test quality.
