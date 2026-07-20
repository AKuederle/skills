# Final Commit Stack

## 1. Establish the Exact Range

Identify the stack base from the actual branch relationship rather than guessing from a branch name. Inspect the complete range in review order:

```bash
git log --reverse --stat <stack-base>..HEAD
git log --reverse --format=fuller <stack-base>..HEAD
git status --short
```

Confirm that implementation is complete and that all required review feedback has been resolved before rewriting the stack.

## 2. Audit Bottom-Up

Review every commit as if it were presented independently to a human reviewer. For each commit, verify:

- It has one coherent purpose.
- Its dependencies appear earlier in the stack.
- It does not depend on a later correction to compile, run, or make sense.
- Behavioral tests accompany the behavior they prove.
- Structural preparation remains behavior-preserving and useful on its own.
- Its message accurately describes the staged outcome and preserves important rationale.
- A reviewer could approve, reject, or revert it without also deciding an unrelated concern.

Then inspect the progression between commits:

- No seam is introduced, casually replaced, and then repaired again.
- Renames and moves are not mixed invisibly with behavior changes.
- Temporary states are intentional, coherent checkpoints rather than implementation debris.
- The dependency order tells the simplest truthful story of the implementation.

## 3. Rewrite Deliberately

Before substantial history surgery, create a recoverable backup reference using a specific, validated name. Then use an interactive rebase or equivalent workflow to:

- Autosquash `fixup!` and `squash!` commits into their targets.
- Reword messages that omit important motivation or contain inaccurate claims.
- Reorder commits into dependency order.
- Split commits containing independently reviewable concerns.
- Combine commits whose separation creates dead code, misleading intermediate states, or review noise.

For example:

```bash
git rebase -i --autosquash <stack-base>
```

Do not compress a coherent multi-commit story into one commit merely to make the graph shorter. Do not preserve noisy checkpoints merely because they were useful during implementation.

If the branch was pushed, inspect its upstream and remote state before rewriting. Rewrite only when branch ownership and the implementation workflow permit it. Update the remote with `--force-with-lease` when required.

## 4. Verify the Rewritten Stack

After the final rewrite:

1. Reinspect the complete graph and messages.
2. Confirm no `fixup!` or `squash!` subjects remain.
3. Run the full relevant verification suite against the rewritten `HEAD`.
4. Run `git diff --check <stack-base>..HEAD` over the final range.
5. Confirm `git status --short` is clean or contains only explicitly preserved unrelated changes.
6. Confirm the final tree still matches the pre-rewrite implementation outcome.

Where the repository's risk and tooling justify it, verify important intermediate commits as well. At minimum, do not leave a commit that is known to be broken or dependent on a later repair.

## Final Gate

Do not declare the stack ready until all statements are true:

- Every requested change appears in the stack.
- Every commit is a meaningful review unit.
- The stack is coherent from bottom to top.
- Messages explain important reasons and decisions.
- Fixups and squash commits have been absorbed.
- Final verification passed after the last rewrite.
- No task-owned changes remain uncommitted.
