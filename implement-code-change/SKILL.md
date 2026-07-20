---
name: implement-code-change
description: Use whenever the user tasks you with creating new code or refactoring existing code in any serious code repository.
---

# Implement Code Changes

## Core Principle

Choose the implementation and verification workflow from the kind of change being made.

## Route the Change

Classify each requested change before writing a test.

Use the behavioral branch only when all of these have clear answers:

1. What behavior important to a user or caller is changing?
2. Through which supported public interface can that behavior be observed?
3. Would the test remain valuable after another internal refactor?
4. Is the behavior important enough to justify permanent test maintenance?

Then follow exactly one route:

- **Behavioral change or bug fix:** Read the standalone [TDD skill](../tdd/SKILL.md) completely and follow it, resolving its relative references from the `tdd/` directory. Do not load the structural-change reference.
- **Structural, mechanical, or trivial change:** Read the standalone [structural-code-change skill](../structural-code-change/SKILL.md) completely and follow it. Do not load the TDD skill.
- **Mixed request:** Separate the behavioral and structural slices. Read and apply each standalone skill only to its corresponding slice. Do not force the whole request through one workflow.

When uncertain, identify the claimed user- or caller-visible outcome. If there is none, use the structural route.
