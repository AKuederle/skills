---
name: structural-code-change
description: Use whenever the user tasks you with simple behavior-preserving refactoring or mechanical code changes in a serious repository, including renaming symbols, moving modules, reorganizing packages, updating imports or exports, removing incidental code, or changing packaging without changing runtime behavior.
---

# Structural Code Changes

Use this workflow for changes that preserve behavior or are checked better by development tooling. Do explicitly not use TDD for trivial changes with obvious outcomes and limited impact. Do not manufacture a red-green cycle.

## Refactor Under Green

1. Run the smallest relevant existing test suite or validation command and establish green.
2. Make the smallest structural or mechanical change.
3. Run the same checks again.
4. Add a test only if the work reveals important behavior that was previously unprotected.

## Do Not Test the Diff

Do not create tests solely to verify:

- File or module names
- Symbol, class, function, or object names
- File locations or directory structure
- The presence or absence of source text, logging calls, comments, or configuration text
- Mechanical import-path changes
- Formatting, spelling, or autocorrect changes
- Generated output mirroring the implementation diff
- Private implementation structure
- Facts already enforced adequately by static tooling

## Choose Direct Verification

Use the most direct relevant check:

- Rename or file move: compiler, type checker, build, and existing tests
- Internal import or export change: compiler, build, and existing tests
- Supported public export change: consumer-facing import or build smoke test when that boundary is a durable contract
- Packaging change: package, install, and consumer import smoke test
- Removal of incidental logging: diff inspection, linting, and existing tests
- Removal of sensitive or contractually forbidden logging: behavioral or security regression test when the absence is itself a durable requirement

For complex cases, you can use inline scripts, temporary test files or script to convince yourself that things work correctly. But for changes classified by this skill, don't persist these validations in the repository.

If the change exposes a genuine behavioral gap, isolate that slice, stop this workflow for it, and use the standalone [TDD skill](../tdd/SKILL.md). Do not apply TDD to the remaining structural work.
