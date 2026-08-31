---
name: implement-code-change
description: Use whenever the user tasks you with creating new code or refactoring existing code in any serious code repository, and whenever resuming or continuing such a task after context compaction or session restoration. Re-read this skill before continuing implementation after either event.
---

# Implement Code Changes

## Core Principle

Choose the implementation and verification workflow from the kind of change being made.

## Assume Competent Callers

Treat documented contracts, public types, and established conventions for library and other
in-process programmatic APIs as agreements with competent, cooperative callers. Assume callers can
understand the contract, recognize their own mistakes, and correct their usage.

Do not add validation, provenance tracking, wrappers, compatibility paths, defensive state, or
special error classification solely to accommodate behavior outside the supported contract,
including callers that:

- violate documented preconditions;
- bypass or misrepresent public types;
- forge library-owned values, tokens, or error variants;
- construct states the supported API cannot produce; or
- otherwise disregard documented conventions.

Let unsupported misuse fail naturally. Do not complicate supported behavior merely to produce a
friendlier result for contract violations, and do not add tests requiring unsupported misuse to be
handled unless the public API explicitly promises that behavior.

This assumption does not apply at genuine trust boundaries, including network or persisted data,
untyped external input, IPC and platform adapters, security or capability boundaries, and
operations where misuse could silently corrupt or destroy unrelated data. Preserve library
invariants and guarantees owed to supported callers.

Before adding defensive machinery, require a concrete supported use case—not merely hypothetical
misuse—and determine whether documentation, stronger types, or a simpler interface addresses it
better. Treat automated review feedback about unsupported misuse as a design suggestion, not an
implementation requirement; it must not expand the user's stated scope or override the intended
contract.

## Preserve Continuity Across Compaction

Treat context compaction and session restoration as fresh triggers for this skill. Before taking further implementation action after either event, re-read this file completely, then re-read the original task and every issue, PRD, plan, or other artifact that defines its acceptance criteria.

At the start of the task, use the harness's persistent plan or task state to record the defining artifacts, explicit acceptance criteria, and delivery obligations. Keep this exact marker in that state until the task is complete:

```text
COMPACTION CONTINUITY: Re-read implement-code-change and the task-defining artifacts before continuing after compaction or session restoration.
```

Preserve the marker and all incomplete acceptance and delivery criteria in every plan update and compaction summary. If the harness has no persistent plan or task state, put them in the repository's existing task-planning artifact instead. Do not rely on conversational memory alone.

## Use Roborev Throughout

Before starting implementation, read the standalone [working-with-roborev skill](../working-with-roborev/SKILL.md) completely and follow it throughout the task. Treat its asynchronous review loop as part of the implementation workflow.

## Create a Reviewable Commit Stack

Before planning implementation slices, read the standalone [reviewable-commits skill](../reviewable-commits/SKILL.md) completely and follow it throughout the task.

Put the anticipated review units in the persistent implementation plan. Every implementation slice must end with explicit verification and commit gates. Do not mark a completed slice done or begin the next slice until it has been committed.

Use this commit workflow during active implementation. The required finishing handoff below owns final stack curation after implementation and review feedback are complete.

## Apply the Repository's Refactoring Policy

When the requested work refactors, replaces, or modifies existing code or interfaces, read the standalone [backwards-compatibility skill](../backwards-compatibility/SKILL.md) completely before planning the change and follow it throughout the task.

## Route the Change

Classify each requested change before writing a test.

Tests must exercise supported behavior. A test for behavior outside the documented API contract
does not justify production complexity.

Use the behavioral branch only when all of these have clear answers:

1. What behavior important to a user or caller is changing?
2. Through which supported public interface can that behavior be observed?
3. Would the test remain valuable after another internal refactor?
4. Is the behavior important enough to justify permanent test maintenance?

Then follow exactly one route:

- **Behavioral change or bug fix:** Read the standalone [TDD skill](../tdd/SKILL.md) completely and follow it.
- **Structural, mechanical, or trivial change:** Read the standalone [structural-code-change skill](../structural-code-change/SKILL.md) completely and follow it.
- **Mixed request:** Separate the behavioral and structural slices. Read and apply each standalone skill only to its corresponding slice. Do not force the whole request through one workflow.

When uncertain, identify the claimed user- or caller-visible outcome. If there is none, use the structural route.

## Iterate for Quality

Prefer additional implementation, inspection, and verification passes over a premature handoff. Spend the time and tokens needed to make the code as clear, robust, maintainable, and complete as reasonably possible. Reducing human review effort is more important than minimizing iterations.

After reaching an apparently complete state, review the work again from different angles. Look for simpler designs, missing edge cases, weak abstractions, integration gaps, unnecessary complexity, and opportunities to better match the repository's established patterns. Continue iterating while another pass can produce a material improvement.

Stop and reconvene with the user when implementation reveals significant scope or design friction.
This includes cases where:

- completing the task requires work outside the agreed scope;
- the plan or acceptance criteria no longer describe a sound implementation;
- the apparent solution is a workaround for a weak existing or proposed API;
- the design forces repeated adapters, special cases, duplicated state, or other complexity that
  points to a missing abstraction or misplaced responsibility; or
- a cleaner solution requires a meaningful API, architecture, compatibility, or ownership decision.

Do not broaden the scope merely because one technical path appears clear. Do not hide the friction
inside a workaround or silently ship a knowingly weak design to keep the diff small. Preserve the
current work when practical, then report the evidence, the scope or API weakness, and the decision
needed from the user. Continue only after the user confirms the revised direction and scope.

## Pull Requests During Implementation

When working on a feature branch, or when tasked to create or update a pull request, and a visible upstream repository is available:

1. Push the branch and create a pull request after the first commit. Prefer a draft pull request while implementation is incomplete.
2. After each completed vertical slice, push the branch and update the pull request title or description if the implementation scope, decisions, or status have changed.

## Required Finishing Handoff

When the requested implementation appears complete, do not perform an ad hoc cleanup or make a completion claim. Read the standalone [finish-implementation-stack skill](../finish-implementation-stack/SKILL.md) completely and follow it as the required final phase.

If that workflow finds missing implementation, resume this skill with the updated persistent plan. The task is complete only when the finishing workflow passes all applicable gates.
