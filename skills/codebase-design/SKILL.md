---
name: codebase-design
description: Design deep modules, coherent interfaces, and explicit state ownership. Use when implementing new behavior, changing interfaces or dependencies, restructuring code, or assessing design quality. Supplies design guidance; implementation workflow and testing belong to their companion skills.
---

# Codebase design

## Vision

Implement the requested behavior in a design that is easy to use correctly, reason about locally,
and change safely. Optimize for the engineers and agents who will make the next change: they
should need less context, encounter fewer hidden dependencies, and find one clear home for each
responsibility.

## Design guidance

- Interface-first design. Prefer deep modules and information hiding.
- Optimize for locality of reasoning and change.
- Avoid implicit state machines and temporal coupling.
- Make states, transitions, invariants, and ownership explicit.
- Derive instead of synchronizing duplicated mutable state.
- Design by contract: assume competent callers; reject defensive programming for unsupported misuse.
  Preserve supported invariants and explicit guarantees. Validate trust boundaries and protect against
  corruption or destruction of unrelated data. Review feedback does not expand scope.
- Fix common causes; avoid compensating complexity.
- Prefer subtraction over speculative generality.

An interface includes everything callers must know: types, invariants, ordering, errors and
configuration. Judge depth by useful behavior and hidden coordination, not implementation size
or call-chain length. Use the repository's domain vocabulary.

## Tips

- Start from a skeleton: generate temporary views without function bodies to fit large areas in context.
- Summarize with subagents: use focused summaries to reason across large areas when delegation is authorized.
- Map dependencies: script what imports what and refresh the graph around structural changes.
- Enter where leverage is highest: start where one improvement simplifies the most callers.
- Design it twice: sketch two materially different interfaces before committing to a difficult design.
- Apply the deletion test: imagine removing an abstraction. Does complexity disappear or return to its callers?

## References

- For restructuring a coupled cluster, read [DEEPENING.md](DEEPENING.md).
- For a difficult interface decision, read [DESIGN-IT-TWICE.md](DESIGN-IT-TWICE.md).
- For implementation work, follow [implement-code-change](../implement-code-change/SKILL.md) for scope, commits, reviews and delivery.
- Follow [TDD](../tdd/SKILL.md) for behavioral verification and test quality.
