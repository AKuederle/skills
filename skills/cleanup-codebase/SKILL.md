---
name: cleanup-codebase
description: Clean up an existing implementation or PR by fixing structural problems and removing obsolete code, comments, tests, and temporary machinery. Use for scoped untangling or finalization cleanup, not unrelated repository-wide redesign.
---

# Clean up a change

Leave the cleanest version of the assigned change, without baggage for future maintainers.
For PR finalization, limit work to this PR and the agent's assigned responsibilities.

Read the accepted requirements, diff, repository guidance, and
[refactor policy](../implement-code-change/references/refactor-policy.md). Use
[codebase-design](../codebase-design/SKILL.md) for design decisions and
[TDD](../tdd/SKILL.md) for test quality. Do not create another delivery workflow.

## Find causes, not symptoms

Trace changed operations through their callers, state, dependencies, and failure/cleanup paths.
Look for hidden coordination, duplicated policy, unnecessary indirection, speculative options,
obsolete paths, and temporary migration machinery. Check the whole migration, including exports,
configuration, documentation, and consumers within the assigned scope.

For a material finding, use this compact refactoring template:

```text
Evidence → supported requirement → common cause → owned fix → verification
Retained exception or deferred work → reason
```

Prefer one fix that removes several symptoms. Preserve supported behavior, security boundaries,
platform constraints, and required compatibility. A special case is a question to investigate,
not proof that code should be removed. Discuss changes beyond the assignment before proceeding.

## Remove the baggage

Implement justified fixes in coherent commits with focused checks. Remove redundant comments and
unused scaffolding. Preserve coverage during restructuring, then rewrite brittle tests and retire
superseded ones according to TDD. Do not weaken failing tests to make cleanup green.

Stop when concrete findings are fixed or justified and checks cover the result; do not invent
work to fill categories. Report changes, retained constraints, verification, and anything that
materially changes the approved design. A delegated cleanup returns to its parent without running
root CI, updating PRs, spawning recursive cleanup, or merging unless explicitly assigned.
