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

Tips: temporary skeleton views; authorized focused subagent summaries; refreshed dependency graphs;
start at highest leverage; compare materially different interfaces; deletion test: does complexity
disappear or return to callers?

For coupled clusters read [DEEPENING](DEEPENING.md); for consequential interface choices read
[DESIGN-IT-TWICE](DESIGN-IT-TWICE.md). [implement-code-change](../implement-code-change/SKILL.md)
owns delivery; [TDD](../tdd/SKILL.md) owns behavior verification/test quality.
