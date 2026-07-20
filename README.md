# Personal Codex skills

This repository contains locally maintained Codex skills.

## `implement-code-change`

The [`implement-code-change`](implement-code-change/) skill is a small router for implementation work in serious repositories. It sends behavioral changes and bug fixes to the standalone TDD skill, sends structural changes to the standalone structural-code-change skill, and splits mixed requests into separate slices.

## `backwards-compatibility`

The [`backwards-compatibility`](backwards-compatibility/) skill is loaded by other refactoring workflows rather than invoked directly. It applies the repository's accepted compatibility policy and requires refactors to improve local understanding and fully replace the superseded design.

## `codebase-design`

The [`codebase-design`](codebase-design/) directory is a faithful copy of the current upstream [codebase-design skill from mattpocock/skills](https://github.com/mattpocock/skills/tree/main/skills/engineering/codebase-design). It provides shared vocabulary and guidance for deep modules, seam placement, testable interfaces, deepening shallow module clusters, and comparing alternative designs. The copy was checked against upstream `main` at commit `9603c1cc8118d08bc1b3bf34cf714f62178dea3b` on 2026-07-20 and remains subject to Matt Pocock's [MIT license](UPSTREAM_LICENSE.md).

## `structural-code-change`

The [`structural-code-change`](structural-code-change/) skill handles behavior-preserving refactoring and mechanical changes under a green baseline. It rejects tests that merely assert names, paths, deleted source text, or other implementation details.

## `setup-repo`

The [`setup-repo`](setup-repo/) skill establishes an explicit backwards-compatibility policy with the user, records the accepted policy in `.agents/refactor-policy.md`, installs and initializes roborev, and encodes the policy into the repository's roborev review guidance.

## `working-with-roborev`

The [`working-with-roborev`](working-with-roborev/) skill coordinates asynchronous roborev feedback throughout implementation. It explains when to inspect or defer feedback, requires dedicated commits for review-driven changes, and closes every review before a feature is considered complete.

## `tdd`

The [`tdd`](tdd/) directory is a faithful copy of the current upstream TDD skill from [mattpocock/skills](https://github.com/mattpocock/skills/tree/main/skills/engineering/tdd), including its test-style, mocking, and OpenAI interface files. The copy was checked against upstream `main` on 2026-07-20.

The original skill was installed with the `skills` CLI from `skills/engineering/tdd/SKILL.md`; that provenance is recorded in `~/.agents/.skill-lock.json` under the `tdd` entry. Upstream material remains subject to Matt Pocock's [MIT license](UPSTREAM_LICENSE.md).
