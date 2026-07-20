---
name: tdd
description: Apply behavior-first test-driven development through small red-green-refactor cycles. Use when the user explicitly requests TDD or red-green-refactor, or when implementing substantive behavior or fixing a bug for which a durable behavioral regression test is valuable. Do not create new tests for pure refactoring, renames, file moves, formatting, packaging mechanics, generated files, or trivial deletions unless they change a meaningful public contract.
---

# Test-Driven Development

## Core Principle

Test meaningful behavior through supported public interfaces. Do not test the implementation diff itself.

Prefer integration-style tests that describe what a user or caller can accomplish. Make tests survive internal refactoring. Avoid tests coupled to private methods, internal collaborators, source text, filenames, or incidental structure.

## Choose the Correct Change Mode

Classify the requested change before writing a test.

### Behavioral change or bug fix

Use red-green-refactor when the change adds, removes, or corrects meaningful externally observable behavior and a permanent regression test is worth maintaining.

Before adding a test, answer all of these:

1. What behavior important to a user or caller is changing?
2. Through which supported public interface can that behavior be observed?
3. Would this test remain valuable after another internal refactor?
4. Is the behavior important enough to justify permanent test maintenance?

If the answers are clear, proceed with a behavioral TDD cycle.

### Structural, mechanical, or trivial change

Do not manufacture a failing test for a change that preserves behavior or is already checked better by development tooling. Refactor under green instead:

1. Run the smallest relevant existing test suite or validation command and establish green.
2. Make the smallest structural or mechanical change.
3. Run the same checks again.
4. Add a test only if the work reveals an important behavior that was previously unprotected.

Do not create tests solely to verify:

- File or module names
- Symbol, class, function, or object names
- File locations or directory structure
- The presence or absence of source text, logging calls, comments, or configuration text
- Mechanical import-path changes
- Formatting, spelling, or autocorrect changes
- Generated output mirroring the implementation diff
- Private implementation structure
- Facts already enforced adequately by the compiler, type checker, linter, build, or package manager

Use the most direct relevant verification instead:

- Rename or file move: compiler, type checker, build, and existing tests
- Internal import or export change: compiler, build, and existing tests
- Supported public export change: consumer-facing import or build smoke test when that boundary is a durable contract
- Packaging change: package, install, and consumer import smoke test
- Removal of incidental logging: diff inspection, linting, and existing tests
- Removal of sensitive or contractually forbidden logging: behavioral or security regression test when the absence is itself a durable requirement

Do not turn an implementation diff into a behavioral contract.

## Plan a Behavioral TDD Cycle

Before beginning a behavioral TDD cycle:

- Confirm the public interface and the behavior priorities with the user when they are not already clear.
- Identify the seams under test: supported public boundaries where behavior can be observed without reaching into internals.
- Confirm those seams with the user when the request does not already establish them. Do not add tests at arbitrary or incidental boundaries.
- Use the project's domain language in test names and interfaces.
- Respect relevant architecture decisions and established test conventions.
- List behaviors, not implementation steps.
- Focus test effort on critical paths and complex logic rather than exhaustive low-value coverage.

## Work in Vertical Slices

Do not write all tests first and then all implementation. Work one behavior at a time:

```text
RED:   Add one test for one missing behavior and observe the expected failure.
GREEN: Add the minimum implementation needed to make that test pass.
REPEAT: Select the next behavior using what the previous cycle taught you.
STOP:  End the implementation loop once the requested behaviors are green.
```

A valid red failure must demonstrate missing or incorrect behavior. A test failing only because an expected filename, symbol name, object name, log statement, or source-code pattern differs is not behavioral evidence.

## Write Durable Tests

Write tests that:

- Exercise real code paths through supported public interfaces
- Describe user- or caller-visible outcomes
- Fail when the protected behavior breaks
- Survive internal restructuring and renaming
- Use one focused logical expectation per behavior

Avoid tests that:

- Mock internal collaborators merely to assert calls or ordering
- Test private methods directly
- Inspect source files for implementation text
- Assert internal shapes that callers do not depend on
- Recompute the expected result using the same logic as the implementation, making the assertion true by construction
- Duplicate guarantees already provided by static tooling
- Exist only to make the current edit produce a red step

Use mocks at true system boundaries when the real dependency is unavailable, unsafe, nondeterministic, or prohibitively slow. Prefer fakes or contract-level boundaries over mocks of internal modules.

Derive expected values from an independent source of truth such as a specification, known-good literal, or worked example.

## Keep Refactoring Outside the Loop

Treat broader refactoring as a separate review stage, not another red-green implementation cycle. After the requested behavior is green, review the implementation and decide whether refactoring is warranted. If it is, improve the implementation in small steps and run the relevant existing tests after each step.

Never refactor while red. Restore green and end the behavioral loop first.

## Cycle Checklist

Before keeping a new test, verify:

- [ ] The test protects meaningful, durable behavior.
- [ ] The test uses a supported public interface.
- [ ] The test would survive an internal refactor or rename.
- [ ] The test is not merely an assertion about the requested diff.
- [ ] Static tooling or an existing test does not already provide the right protection.
- [ ] The maintenance cost is justified by the regression risk.

If any answer is no, revise the test or remove it.
