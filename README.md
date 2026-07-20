# Personal Codex skills

This repository contains locally maintained Codex skills.

## `implement-code-change`

The [`implement-code-change`](implement-code-change/) skill is a small router for implementation work in serious repositories. It sends behavioral changes and bug fixes to the standalone TDD skill, sends structural changes to the standalone structural-code-change skill, and splits mixed requests into separate slices.

## `structural-code-change`

The [`structural-code-change`](structural-code-change/) skill handles behavior-preserving refactoring and mechanical changes under a green baseline. It rejects tests that merely assert names, paths, deleted source text, or other implementation details.

## `tdd`

The [`tdd`](tdd/) directory is a faithful copy of the current upstream TDD skill from [mattpocock/skills](https://github.com/mattpocock/skills/tree/main/skills/engineering/tdd), including its test-style, mocking, and OpenAI interface files. The copy was checked against upstream `main` on 2026-07-20.

The original skill was installed with the `skills` CLI from `skills/engineering/tdd/SKILL.md`; that provenance is recorded in `~/.agents/.skill-lock.json` under the `tdd` entry. Upstream material remains subject to Matt Pocock's [MIT license](UPSTREAM_LICENSE.md).
