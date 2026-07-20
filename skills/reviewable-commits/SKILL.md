---
name: reviewable-commits
description: Use whenever committing changes during implementation, deciding commit boundaries, creating fixup or squash commits, organizing an existing change set, or preparing a branch's final commit stack for human review.
---

# Create Reviewable Commits

## Core Contract

Treat commits made during implementation as the future review stack. Commit early for safety, but choose boundaries that can survive into the final history. Do not rely on final cleanup to recover logical boundaries from one large implementation commit.

Use one continuous workflow:

```text
Plan commit boundaries
-> implement one slice
-> verify the slice
-> commit the slice
-> record follow-ups as fixup or squash commits
-> repeat
-> curate the final stack
-> verify the rewritten stack
```

## Required Final State

The final commit stack must:

- Tell a coherent, dependency-ordered story.
- Be reviewable and approvable commit by commit from the bottom up.
- Keep every commit internally consistent, independently understandable, and safe to check out.
- Keep behavioral tests with the change whose behavior they prove.
- Separate unrelated changes and distinguish preparatory refactors from behavior changes.
- Contain no unresolved `fixup!` or `squash!` commits.
- Avoid later commits repeatedly rewriting seams introduced earlier unless the stack intentionally demonstrates a meaningful multi-step transition.
- Use messages that preserve important motivation and decisions that cannot be derived from the diff.

These are design constraints during implementation, not merely cleanup checks at the end.

## Establish the Commit Plan

Before the first implementation commit:

1. Inspect the working tree and recent repository history.
2. Identify the stack base and distinguish pre-existing user changes from task changes.
3. Divide the requested work into logical, dependency-ordered review units.
4. Put each anticipated commit in the persistent implementation plan.
5. Give every unit an explicit verification-and-commit gate.

Do not use arbitrary file or line limits as commit boundaries. A commit is one coherent change that a reviewer could approve, reject, revert, and understand independently.

## Work During Implementation

Before creating the first commit, read [references/during-implementation.md](references/during-implementation.md) completely and follow it for every implementation slice.

Do not begin the next implementation slice while the current completed slice remains uncommitted. If the current work is not yet a coherent review unit, it is still the current slice; do not silently start another concern.

Keep corrections and review feedback recoverable during active implementation:

- Use `git commit --fixup=<target>` when a follow-up belongs entirely in an earlier commit and needs no retained message context.
- Use `git commit --squash=<target>` when the follow-up belongs in an earlier commit but its message contains context worth preserving.
- Create a normal commit when the change is an independently reviewable unit.
- Never amend a reviewed commit directly while the active review workflow requires dedicated fixups.

## Curate the Final Stack

After implementation and review feedback are complete, read [references/final-stack.md](references/final-stack.md) completely and follow it before final verification or completion claims.

This is a hard transition gate. Do not declare the task complete until the final stack has been curated, rewritten as needed, and verified in its rewritten form.

## Safety

- Preserve unrelated and pre-existing user changes.
- Stage only the intended paths and hunks for the current commit.
- Do not rewrite shared history without first verifying branch ownership and remote state.
- When rewriting a pushed branch is authorized and safe, use `--force-with-lease`, never an unconditional force push.
- Create a recoverable backup reference before substantial stack surgery.
- Do not use destructive Git commands merely to make history cleanup easier.
