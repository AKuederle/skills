---
name: finish-implementation-stack
description: Use whenever an implementation appears complete, before claiming implementation work is finished, or when resuming a task in its final review, commit-stack, verification, push, or pull-request stage.
---

# Finish an Implementation Stack

## Completion Gate

Turn an apparently complete implementation into a verified, review-ready delivery.

```text
Reconstruct the task contract
-> prove implementation completeness
-> resolve asynchronous reviews
-> curate the commit stack
-> review the final stack
-> append review corrections without rewriting
-> verify the final result
-> deliver the branch and pull request
-> report evidence
```

Do not make a completion claim until every applicable implementation, commit-stack, review, verification, push, and pull-request obligation has fresh evidence.

## 1. Reconstruct the Task Contract

Re-read the original user request and every artifact that defined the implementation, including issues, PRDs, plans, acceptance criteria, design documents, linked discussions, and explicit decisions made with the user.

Read the persistent implementation plan or task state. Recover:

- The complete acceptance-criteria checklist.
- The stack base and intended review units.
- The current branch, upstream, and pull request.
- Required verification commands.
- Incomplete delivery obligations.
- The finalized stack head and final review job, if the final review has already started.

Preserve any compaction-continuity marker until this workflow finishes.

## 2. Prove the Implementation Is Complete

Check every acceptance criterion against the actual implementation. Inspect the final tree and diff for missing integrations, unfinished follow-ups, stale TODOs, obsolete paths, partial migrations, and requirements that tests do not prove.

If anything is missing, update the persistent plan and return to the [implement-code-change skill](../implement-code-change/SKILL.md). Do not continue the finishing workflow around an incomplete implementation.

Continue only when the requested implementation is materially complete.

## 3. Resolve Asynchronous Reviews

Read the [working-with-roborev skill](../working-with-roborev/SKILL.md) completely and finish its review cycle for every implementation commit:

1. Wait for relevant pending reviews.
2. Inspect and address valid findings in dedicated fixup commits before final review. If final
   review has already started, preserve the reviewed stack and use normal additional commits.
3. Comment on and close every review, including reviews with no relevant findings.
4. Repeat until no relevant per-commit review remains open.

Do not treat an empty query caused by an incorrect range, repository, or status filter as proof that reviews are complete.

## 4. Curate and Review the Final Stack

Read the [reviewable-commits skill](../reviewable-commits/SKILL.md) and its [final-stack procedure](../reviewable-commits/references/final-stack.md) completely. Before starting final review, absorb implementation fixups, improve boundaries and messages, and verify bottom-up reviewability. Do not repeat curation when resuming after final review has started.

After curation, follow the [final review procedure](../working-with-roborev/references/final-review.md).
Invoke the configured `feature_ready` panel against the exact full task or PR range. If it is not
configured, use a single whole-stack review. Record the finalized stack head before enqueueing.
This starts the phase in which history must be preserved.

If the final review produces relevant findings:

1. Assess each finding against the accepted requirements and actual code.
2. Address valid findings in normal additional commits, with messages explaining the correction.
3. Comment on and close the final review, including dispositions for rejected findings.
4. Resolve automatic reviews of correction commits with further normal additional commits.
5. Continue to fresh verification of the complete result.

Do not amend, rebase, autosquash, reorder, or squash the reviewed stack or its correction commits.
Retain the additional commits so the user can assess what the final review changed. This rule
also applies after compaction, retries, or a return to implementation prompted by final review.

Do not request repeated whole-stack reviews merely to review fixes from the final whole-stack review. Ensure no Roborev job for the implementation remains open before continuing.

## 5. Run Fresh Final Verification

After the last correction commit:

1. Recheck every acceptance criterion against the final result.
2. Inspect the final diff and commit graph over the exact stack range.
3. Run the full relevant test, type-check, lint, formatting, build, packaging, and repository-specific verification commands.
4. Check the worktree for formatter output or uncommitted task-owned changes.
5. Confirm there are no unresolved `fixup!` or `squash!` commits.
6. Confirm the finalized pre-review head remains an ancestor and its commits were not rewritten.

Do not infer full success from partial checks or verification run before the final correction.

## 6. Deliver the Branch and Pull Request

When the work is on a feature branch, or a pull request was requested, and a visible upstream repository is available:

1. Inspect branch tracking and the current remote state.
2. Push the final stack with its retained correction commits. If pre-review curation rewrote an
   owned, previously pushed stack, use `--force-with-lease`, never an unconditional force push.
3. Ensure a pull request exists. Create it if the active implementation workflow required one but it is missing.
4. Update the pull-request title and description to match the final stack.
5. Include relevant issue, PRD, plan, ADR, and other source-artifact links.
6. Record important decisions, breaking changes, migration guidance, and verification evidence.
7. Inspect the resulting pull request and confirm its head, base, title, body, and state are correct.

Do not merge the pull request unless the user explicitly requested the merge. If delivery is not applicable or impossible, record the concrete reason rather than silently skipping it.

## 7. Report Completion Evidence

Report concisely:

- What was implemented.
- The final commit stack.
- The final panel or fallback review job, finalized pre-review head, and additional correction commits.
- Acceptance-criteria coverage.
- Verification commands and results.
- Roborev closure state.
- Branch push and pull-request state, including the pull-request link when applicable.
- Any intentionally deferred work or explicit blocker.

Only then remove the compaction-continuity marker and declare the implementation complete.
