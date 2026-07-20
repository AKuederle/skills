---
name: implement-code-change
description: Use whenever the user tasks you with creating new code or refactoring existing code in any serious code repository, and whenever resuming or continuing such a task after context compaction or session restoration. Re-read this skill before continuing implementation after either event.
---

# Implement Code Changes

## Core Principle

Choose the implementation and verification workflow from the kind of change being made.

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

After all implementation and Roborev feedback are complete, follow the skill's final-stack procedure before final verification. Treat commit-stack quality as a delivery obligation, not an optional cleanup pass.

## Apply the Repository's Refactoring Policy

When the requested work refactors, replaces, or modifies existing code or interfaces, read the standalone [backwards-compatibility skill](../backwards-compatibility/SKILL.md) completely before planning the change and follow it throughout the task.

## Route the Change

Classify each requested change before writing a test.

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

When the most elegant solution is blocked by an adjacent problem outside the original scope, make an executive decision:

- If there is one clear and logical way to resolve it, the added scope is reasonable, and the change remains safe and consistent with the repository's architecture, implement the additional change and record the decision in the relevant commit message.
- If multiple materially different solutions exist, the expansion is substantial or risky, or it requires authority or information you do not have, stop the implementation and report the blocker together with the decision required from the user.

Do not silently settle for a knowingly inferior solution merely to keep the diff small.

## Final Verification

Before finishing the task, verify that the complete requested implementation is present in the final stack:

1. Re-read the original user request and every artifact used to define the work, including plan documents, GitHub issues, PRDs, acceptance criteria, design documents, and linked discussions.
2. Turn the requirements from those sources into a checklist and verify each item against the implementation. Do not treat passing tests as proof that every requested requirement was implemented.
3. Confirm the final-stack gate from the [reviewable-commits skill](../reviewable-commits/SKILL.md) was completed after implementation and Roborev feedback ended.
4. Inspect the final diff and commit graph for omissions, unfinished follow-ups, stale TODOs, or partial implementation paths.
5. Run the full relevant verification commands against the cleaned-up final stack.
6. Finish only when confident that everything requested has been implemented and verified. If anything remains incomplete or cannot be verified, continue working or report the specific blocker instead of declaring completion.

## Pull Requests

When working on a feature branch, or when tasked to create or update a pull request, and a visible upstream repository is available:

1. Push the branch and create a pull request after the first commit. Prefer a draft pull request while implementation is incomplete.
2. After each completed vertical slice, push the branch and update the pull request title or description if the implementation scope, decisions, or status have changed.
3. Keep the pull request description aligned with the finished commit stack and include relevant issue or planning-document links.
