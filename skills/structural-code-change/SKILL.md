---
name: structural-code-change
description: Verify mechanical, behavior-preserving edits such as renames, moves, imports/exports, packaging, and incidental-code removal without manufactured TDD.
---

# Structural changes

Read the [refactor policy](../implement-code-change/references/refactor-policy.md) before
planning/editing; [implement-code-change](../implement-code-change/SKILL.md) owns
commits/reviews/delivery. Establish green with the smallest relevant existing check, make the
change, rerun. Add coverage only for meaningful unprotected behavior discovered.

Do not persist tests for names/paths, source or config text, mechanical imports, formatting,
mirrored generated output, private structure, or facts already enforced statically. Temporary probes
can support diagnosis.

Use compiler/typecheck/build and existing tests for renames/moves/internal exports; durable public
exports may need consumer import/build smoke tests. Packaging needs package/install/consumer
verification. Incidental log removal needs inspection/lint/existing tests; forbidden sensitive
logging may need a behavioral/security regression. If behavior changes, isolate that slice under
[TDD](../tdd/SKILL.md); do not impose it on remaining structural work.
