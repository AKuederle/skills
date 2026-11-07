# Reviewable commits

Inspect worktree, base, history, and ownership. Preserve unrelated changes. Commit coherent,
dependency-ordered review units with behavior, tests, and required migrations together; separate
independent concerns, not file categories. Verify and inspect staged changes before committing;
check leftovers and record evidence. Messages explain non-obvious motivation.

Before first full-range review, curate checkpoints and implementation fixups into a coherent
bottom-up stack without known broken commits or dead scaffolding. User-requested corrections always
remain separate follow-ups. After range review, append all corrections, including automatic-review
fixes; do not fold or reorder them into earlier units.

Parent rebases remain allowed: preserve review units/correction intent, inspect the range diff,
record new base/head, and revalidate affected checks/reviews. Back up substantial rewrites; check
remote ownership; use `--force-with-lease` for authorized pushed-history rewrites. Verify intended
tree and units survived.
