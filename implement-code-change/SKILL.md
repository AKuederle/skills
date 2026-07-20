---
name: implement-code-change
description: Use whenever the user tasks you with creating new code or refactoring existing code in any serious code repository.
---

# Implement Code Changes

## Core Principle

Choose the implementation and verification workflow from the kind of change being made.

## Route the Change

Classify each requested change before writing a test.

Use the behavioral branch only when all of these have clear answers:

1. What behavior important to a user or caller is changing?
2. Through which supported public interface can that behavior be observed?
3. Would the test remain valuable after another internal refactor?
4. Is the behavior important enough to justify permanent test maintenance?

Then follow exactly one route:

- **Behavioral change or bug fix:** Read the standalone [TDD skill](../tdd/SKILL.md) completely and follow it, resolving its relative references from the `tdd/` directory. Do not load the structural-change reference.
- **Structural, mechanical, or trivial change:** Read the standalone [structural-code-change skill](../structural-code-change/SKILL.md) completely and follow it. Do not load the TDD skill.
- **Mixed request:** Separate the behavioral and structural slices. Read and apply each standalone skill only to its corresponding slice. Do not force the whole request through one workflow.

When uncertain, identify the claimed user- or caller-visible outcome. If there is none, use the structural route.

## Commit Hygiene

Treat commits as both recoverable checkpoints during implementation and review units in the final stack.

### Commit During Implementation

- Commit early and regularly in logical vertical slices. Each commit should leave the repository in a stable state and provide a useful point to return to if later work fails.
- Prefer a complete thin path through the affected behavior over horizontal commits that separately add types, tests, and implementation scaffolding.
- Each commit automtiaclly triggeres an autoreviewer (see below)

### Mark Follow-Up Commits for Autosquash

When a commit is a clear correction, completion, or second part of an earlier commit rather than an independent review unit, target the earlier commit explicitly:

- Use `git commit --fixup=<target>` when the follow-up changes should be folded into the target and do not need a separate commit message.
- Use `git commit --squash=<target>` when the changes belong in the target but the follow-up message contains context that should be retained when the messages are combined.
- Do not mark a genuinely independent vertical slice as a fixup or squash commit.

For example, after fixing an issue in commit `abc1234`:

```bash
git add path/to/affected-files
git commit --fixup=abc1234
```

Keep fixup and squash commits during active implementation so they remain useful checkpoints. Fold them into their targets during final stack cleanup with an autosquash rebase:

```bash
git rebase -i --autosquash <stack-base>
```

### Write Context-Rich Messages

Use a concise subject and a multi-line body whenever relevant context cannot be derived directly from the diff. Explain why the change was made more than what changed.

Include when applicable:

- Implementation decisions made with the user, especially details where multiple reasonable choices existed
- Constraints, tradeoffs, or rejected alternatives that explain the chosen design
- Links to issues, PRDs, planning documents, ADRs, or other material used as the basis for the implementation

Do not narrate obvious file-level changes that the diff already communicates.

### Preserve Seam Progression

Later commits in the same stack should not repeatedly change the same seams. When a seam must evolve across commits, make that progression an explicit multi-step refactor with independently coherent checkpoints.

Make the stack reviewable and approvable commit by commit from the bottom up. Each commit should have a clear purpose, manageable size, and no hidden dependency on a later cleanup commit.

### Clean Up Before Completion

After completing a coherent section of the implementation, such as a complete feature, inspect the commit graph and optimize it for human review:

- Split commits that contain unrelated concerns.
- Squash or fix up commits that merely repair earlier commits in the same logical slice.
- Reorder commits when doing so makes dependencies and intent clearer.
- Use interactive rebase or an equivalent fixup/autosquash workflow before declaring the work complete.
- Re-run the relevant checks after rewriting the stack.

This cleanup requirement must not discourage committing early and often during implementation.

Do not rewrite shared history blindly. If the stack has already been pushed, verify branch ownership and the current remote state before rewriting it. Use `--force-with-lease`, never an unconditional force push, and do not overwrite unexpected remote commits.

## Pull Requests

When working on a feature branch, or when tasked to create or update a pull request, and a visible upstream repository is available:

1. Push the branch and create a pull request after the first commit. Prefer a draft pull request while implementation is incomplete.
2. After each completed vertical slice, push the branch and update the pull request title or description if the implementation scope, decisions, or status have changed.
3. Keep the pull request description aligned with the finished commit stack and include relevant issue or planning-document links.
