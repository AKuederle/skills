# Reviewable commits

Inspect the worktree, branch/base, and recent history. Preserve unrelated changes. Plan coherent,
dependency-ordered units that a reviewer can understand, approve, and revert independently.

- Keep behavior, its tests, and required consumer migrations together.
- Separate independent preparation or fixes; avoid commits divided only by file category.
- Verify each completed unit, inspect its staged diff, and commit before accumulating another concern.
- Use concise messages; preserve motivation and consequential decisions that the diff cannot explain.
- Check for formatter leftovers and record the commit and verification in task state.

## Curation and corrections

Before the first full-range review, curate implementation checkpoints into a coherent bottom-up
stack. Fixups may absorb implementation repairs, but **user-requested corrections always remain
normal follow-up commits**. Do not bury them during curation. Avoid knowingly broken intermediate
commits, unresolved fixups, and temporary scaffolding with no independent purpose.

Once full-range review starts, preserve the reviewed units and append corrections as normal commits,
including corrections from their automatic reviews. Do not amend or squash those corrections into
earlier commits or one another.

This preserves review history, not commit hashes. Rebasing onto an updated parent remains allowed.
Inspect the range diff after a rebase, preserve commit boundaries and correction intent, and
revalidate changed resolutions. Record the new base/head and any invalidated review evidence.

Before substantial rewrites, create a backup ref and inspect branch ownership and remote state.
Use `--force-with-lease` for an authorized rewrite of a pushed branch. After curation, inspect the
stack bottom-up, check the final diff, and verify that the intended tree and review units survived.
