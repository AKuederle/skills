---
name: cleanup-codebase
description: Untangle an existing implementation or PR and remove obsolete code, comments, tests, and temporary machinery within the assigned scope.
---

# Clean up a change

Leave the cleanest assigned change without future baggage. Finalization cleanup stays within this PR
and assigned responsibilities. Read requirements, diff, repository guidance, and
[refactor policy](../implement-code-change/references/refactor-policy.md); use
[codebase-design](../codebase-design/SKILL.md) and [TDD](../tdd/SKILL.md), not another delivery
workflow.

Trace changed operations through callers, dependencies, state, failure, and release. Find hidden
coordination, duplicated policy, unnecessary indirection, speculative options, and obsolete
migration paths; include in-scope exports/config/docs/consumers.

Record material findings as: evidence → supported requirement → common cause → owned fix →
verification; retained/deferred exceptions → reason. Prefer common-cause fixes. Preserve supported
behavior, trust/platform constraints, and compatibility; special cases require investigation, not
automatic deletion. Discuss scope expansion.

Make coherent commits with focused checks. Remove redundant comments and unused scaffolding;
preserve coverage while restructuring, then rewrite brittle/retire superseded tests. Do not weaken
failing tests to get green. Stop when concrete findings are fixed/justified and verified. Report
changes, constraints, evidence, and material design changes. Delegated cleanup returns to its
parent; no root CI, PR updates, recursive cleanup, or merge unless assigned.
