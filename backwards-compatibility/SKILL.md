---
name: backwards-compatibility
description: Use only when another skill explicitly references this skill while refactoring, replacing, or modifying existing code or interfaces. Do not invoke it directly.
---

# Apply the Repository's Backwards-Compatibility Policy

## Load Local Guidance First

Before planning or editing a refactor, read `./.agents/refactor-policy.md` completely.

If the file does not exist, stop the refactoring task. Tell the user that the repository has no accepted backwards-compatibility policy and help them set it up with `$setup-repo`. Do not infer a policy or continue refactoring until setup is complete.

Treat the local policy as fundamental implementation guidance. It dictates whether to make hard cuts, preserve interfaces, introduce migrations, or retain compatibility layers. Only an explicit user instruction may override it, and only for the specific feature or change for which the user opts out. Do not generalize a scoped exception to the rest of the repository or silently rewrite the repository policy.

## Improve Local Understanding

Every refactor must reduce local complexity or make the affected code materially easier to understand. If it does neither, stop and discuss the approach with the user before continuing.

Treat these outcomes as code smells that might require another design pass:

- The affected implementation contains more non-comment lines after the refactor, even when the increase came from extracting a primitive. Do not count comments, and do not game the comparison through compressed formatting.
- A reader must jump through more implementations or files to understand the basic functionality of the affected code.

An extracted abstraction is not automatically an improvement. Re-evaluate its seam, depth, naming, and locality when either smell appears. Proceed only when the resulting design is still clearly easier to understand and any added structure is justified by behavior, compatibility requirements, or a genuinely reusable concept.

## Complete the Replacement

For every part of an implementation that replaces or modifies existing code or interfaces, iterate for as long as required to finish the refactor completely.

- Find and update every affected in-repository consumer.
- Search for old names, imports, exports, call paths, implementations, tests, documentation, configuration, and packaging references.
- Remove obsolete paths, duplicated implementations, abandoned adapters, and stale compatibility shims unless the local policy explicitly requires them.
- Preserve only the compatibility surface required by the accepted policy or a feature-specific user override.
- Repeat inspection and verification until no unintended remnant of the previous design remains.

Do not declare the refactor complete merely because tests pass. Verify that the repository reflects one coherent design and that anything intentionally retained is required and documented.
