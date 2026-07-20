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
-> verify the rewritten result
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

Preserve any compaction-continuity marker until this workflow finishes.

## 2. Prove the Implementation Is Complete

Check every acceptance criterion against the actual implementation. Inspect the final tree and diff for missing integrations, unfinished follow-ups, stale TODOs, obsolete paths, partial migrations, and requirements that tests do not prove.

If anything is missing, update the persistent plan and return to the [implement-code-change skill](../implement-code-change/SKILL.md). Do not continue the finishing workflow around an incomplete implementation.

Continue only when the requested implementation is materially complete.

## 3. Resolve Asynchronous Reviews

Read the [working-with-roborev skill](../working-with-roborev/SKILL.md) completely and finish its review cycle for every implementation commit:

1. Wait for relevant pending reviews.
2. Inspect and address valid findings in dedicated fixup commits.
3. Comment on and close every review, including reviews with no relevant findings.
4. Repeat until no relevant per-commit review remains open.

Do not treat an empty query caused by an incorrect range, repository, or status filter as proof that reviews are complete.

## 4. Curate and Review the Final Stack

Read the [reviewable-commits skill](../reviewable-commits/SKILL.md) and its [final-stack procedure](../reviewable-commits/references/final-stack.md) completely. Follow the procedure to absorb fixups, improve boundaries and messages, and verify bottom-up reviewability.

After curation, request the final whole-stack Roborev review using the exact stack base. If it produces relevant findings:

1. Address them in dedicated fixup commits targeting the commits that introduced the problems.
2. Comment on and close the review.
3. Run final-stack curation again so the new fixups are absorbed.
4. Continue to fresh verification against the newly rewritten stack.

Do not request repeated whole-stack reviews merely to review fixes from the final whole-stack review. Ensure no Roborev job for the implementation remains open before continuing.

## 5. Run Fresh Final Verification

After the last code edit and history rewrite:

1. Recheck every acceptance criterion against the rewritten result.
2. Inspect the final diff and commit graph over the exact stack range.
3. Run the full relevant test, type-check, lint, formatting, build, packaging, and repository-specific verification commands.
4. Check the worktree for formatter output or uncommitted task-owned changes.
5. Confirm there are no unresolved `fixup!` or `squash!` commits.

Do not infer full success from partial checks or verification run before the final rewrite.

## 6. Deliver the Branch and Pull Request

When the work is on a feature branch, or a pull request was requested, and a visible upstream repository is available:

1. Inspect branch tracking and the current remote state.
2. Push the final rewritten stack. If an owned, previously pushed stack was rewritten, use `--force-with-lease`, never an unconditional force push.
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
- Acceptance-criteria coverage.
- Verification commands and results.
- Roborev closure state.
- Branch push and pull-request state, including the pull-request link when applicable.
- Any intentionally deferred work or explicit blocker.

Only then remove the compaction-continuity marker and declare the implementation complete.
