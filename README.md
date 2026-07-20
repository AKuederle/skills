# Personal Codex skills

This repository contains locally maintained Codex skills.

## `tdd`

The [`tdd`](tdd/) skill guides agents through behavior-first red-green-refactor cycles. It distinguishes substantive behavioral work from structural, mechanical, and trivial changes so agents do not create permanent test noise merely to prove that a rename, file move, deleted log statement, or similar edit occurred.

This is an adapted version of the TDD skill from [mattpocock/skills](https://github.com/mattpocock/skills/tree/main/skills/engineering/tdd). The original was installed with the `skills` CLI from `skills/engineering/tdd/SKILL.md`; that provenance is recorded in `~/.agents/.skill-lock.json` under the `tdd` entry.

The adaptation was compared with [the current upstream `SKILL.md`](https://github.com/mattpocock/skills/blob/main/skills/engineering/tdd/SKILL.md) on 2026-07-20. It retains upstream's newer guidance on test seams, tautological tests, vertical slices, and keeping refactoring outside the red-green loop, while adding a stricter applicability gate for structural and trivial changes.
