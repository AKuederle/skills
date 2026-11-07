---
name: implement-code-change
description: Coordinate feature development and refactoring from alignment through implementation, review, feedback, and user-triggered finalization. Use for serious code changes and when resuming their review or delivery.
---

# Implement code changes

Use these phases as a working model, scaling effort to scope and risk. Feedback can arrive at any
point. Ready for user review and ready to merge are separate decisions.

## 1. Align

Read repository guidance, code, and task artifacts before asking questions. Resolve consequential
ambiguities with the user until the outcome and workplan are clear. Treat necessary coordinated
changes as in scope; improve repository-owned APIs and consumers together. Discuss large expansions.

Record acceptance criteria, scope, branch/base, verification evidence, and remaining obligations in
existing task state. After compaction or restoration, re-read this skill and the task-defining
artifacts before continuing. Resume the recorded phase rather than restarting completed work.

## 2. Implement and commit

Choose the relevant companion guidance:

- Design or interface decisions: [codebase-design](../codebase-design/SKILL.md).
- Changes to existing code or interfaces: [refactor policy](references/refactor-policy.md).
- Behavioral changes and fixes: [TDD](../tdd/SKILL.md).
- Mechanical, behavior-preserving work: [structural-code-change](../structural-code-change/SKILL.md).

For mixed work, apply each route to its slice. Read [commit guidance](references/commits.md) and
[review mechanics](references/roborev.md) before starting. Commit coherent units often.
With a remote repository, push and create a draft PR
early, then keep its scope, description, and branch current.

## 3. Verify proportionately

Run applicable formatting, lint, static checks, and tests for the changed scope and consumers.
Run long tests during implementation when the change could affect their paths; defer full
verification to the later review and finalization boundaries. Follow repository requirements.

Reuse passing evidence while its inputs remain unchanged. Record what invalidates it; neither a
new message nor a changed commit hash alone requires a rerun. Build the lever for repetitive
edits: verify one scripted transformation before applying it broadly.

## 4. Review each commit

Use RoboRev throughout. Check asynchronous reviews at natural pauses,
assess findings against the accepted task, and resolve relevant jobs before declaring readiness.

## 5. Ready for user review

Check the acceptance criteria, curate a coherent reviewable stack, and run the appropriate broader
verification. Request a full-range review using the configured panel, or normal RoboRev when no
panel exists. Address concerns, push, update the PR, and tell the user what is ready to review,
with verification evidence and any limitations. This does not authorize finalization or merge.

## 6. Integrate feedback

User-requested corrections always go into separate follow-up commits, even before phase 5.
Preserve corrections after full-range review too. Parent-branch rebases remain allowed; preserve
review units and rerun checks or reviews invalidated by conflict resolutions or changed inputs.

## 7. Finalize only when asked

When the user asks to finalize and merge, follow [finalization](references/finalization.md):
delegate cleanup limited to this PR and the agent's assigned responsibilities, remove disposable
artifacts, request final full-range review, finalize the PR description, run final CI, and merge.
Aim for the cleanest version of this change, without baggage for future maintainers. Judge whether
resulting changes warrant renewed user approval; explain material changes when they do.

Delegated workers own their assigned slice and focused evidence. The integration owner coordinates
cross-slice changes, stack curation, root verification, PRs, and merge.
