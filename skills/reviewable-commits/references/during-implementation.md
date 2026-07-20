# Commits During Implementation

## 1. Inspect Before Planning

Run the repository-appropriate equivalents of:

```bash
git status --short
git diff --stat
git diff
git log --oneline --decorate -20
```

Read repository instructions and recent commits touching the affected area. Follow the repository's established subject format unless the user explicitly requests another convention.

Record:

- The stack base.
- Pre-existing changes that must not be included.
- The anticipated commit sequence.
- The behavior or structural outcome proved by each commit.
- The verification required before each commit.

## 2. Choose Review Units

Prefer thin, complete vertical slices. A review unit should carry its own reason to exist and leave the repository in a coherent state.

Split changes when:

- They implement unrelated outcomes.
- A behavior-preserving preparation can stand alone from the behavior change it enables.
- A pre-existing bug fix is independent from the new feature that exposed it.
- A reviewer could reasonably approve one part while rejecting another.
- Combining them would obscure an important design decision or make reversal unsafe.

Keep changes together when:

- A test proves the behavior introduced by the same change.
- Splitting would require dead code, speculative scaffolding, temporary compatibility seams, or a knowingly broken intermediate state.
- The changes are inseparable parts of one small behavioral or structural outcome.

Do not create horizontal commits merely for file categories such as types, tests, implementation, or documentation. Put supporting material in the commit whose outcome requires it.

## 3. Complete the Slice Gate

Before committing a slice, verify all of the following:

- The slice does one logical thing and does it completely.
- Its relevant tests, checks, or structural verification pass after the last edit.
- The repository is not knowingly broken at this point in the stack.
- The staged diff contains exactly this slice and no unrelated changes.
- The commit will make sense without knowledge of later commits.

Inspect the staged change:

```bash
git diff --cached --stat
git diff --cached
```

If the gate fails, finish or correct the current slice. Do not start the next slice.

## 4. Write the Message

Use a concise subject matching repository conventions. Prefer an imperative description of the outcome.

Add a multi-line body when it preserves context the diff cannot communicate, including:

- Why the change was necessary.
- Decisions made with the user.
- Constraints and tradeoffs.
- Rejected alternatives when they explain the design.
- Links to issues, PRDs, plans, ADRs, or other source artifacts.

Do not narrate file operations already obvious from the diff.

For a non-trivial bug fix, explain:

1. The failure and the conditions that triggered it.
2. The correction.
3. Why this correction is preferable to the alternatives.

Example:

```bash
git commit \
  -m "Prevent stale discovery claims during startup" \
  -m "Keep profile ownership with the daemon lifetime so a slow health check cannot allow a second process to take over. This preserves the single-owner guarantee described in issue #123."
```

## 5. Classify Follow-Ups Correctly

Use a fixup when a correction belongs to an earlier review unit:

```bash
git add path/to/affected-files
git commit --fixup=<target-commit>
```

Use a squash commit when its message contains context that should be combined with the target message:

```bash
git add path/to/affected-files
git commit --squash=<target-commit>
```

If one correction belongs to multiple target commits, split it by target. Do not hide unrelated work inside a convenient fixup.

## 6. Close the Slice

After committing:

1. Inspect `git status --short` for formatter output, forgotten changes, or accidental leftovers.
2. Record the commit hash and completed verification in the persistent plan.
3. Start or inspect any repository-required asynchronous review.
4. Push and update the pull request when the implementation workflow requires it.
5. Only then move to the next slice.

## Red Flags

- Multiple completed concerns accumulating before the first commit.
- “I will split it later.”
- A commit that only adds unused scaffolding for a later commit.
- Tests separated from the targeted behavior they prove.
- Generic messages such as `update`, `misc fixes`, or `address review`.
- Later commits repeatedly repairing or renaming the same new seam.
- Treating a passing test suite as proof that the commit boundary is good.
