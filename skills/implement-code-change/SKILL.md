---
name: implement-code-change
description: Coordinate serious code changes through alignment, implementation, review, feedback, and user-triggered finalization. Resume the recorded phase after interruptions.
---

# Implement code changes

Scale effort to scope and risk. Feedback can arrive in any phase.

When delegated, apply this guidance within your assignment; do not independently restart the main
workflow. Unless explicitly assigned otherwise, the parent owns cross-slice integration, stack
curation, full verification, PRs, readiness/finalization reviews, and merge. Return a focused
handoff.

1. **Align.** Inspect code, repository guidance, and task artifacts before asking questions. Resolve
   consequential ambiguity until outcome and workplan are clear. Necessary coordinated API/consumer
   changes are in scope; discuss large expansions. Keep acceptance criteria, scope, base/head,
   verification, and outstanding obligations in existing task state. After restoration, re-read this
   skill and task artifacts; resume rather than restart.
2. **Implement and commit.** Route design to [codebase-design](../codebase-design/SKILL.md),
   existing-code changes to the [refactor policy](references/refactor-policy.md), behavior to
   [TDD](../tdd/SKILL.md), and mechanical work to
   [structural-code-change](../structural-code-change/SKILL.md); split mixed work accordingly. Read
   [commit](references/commits.md)/[review](references/roborev.md) mechanics before starting. Commit
   coherent units often; create a draft PR early when a remote exists and keep it current.
3. **Verify proportionately.** Follow repository requirements. Run relevant static checks and tests
   for changed paths and consumers; run long tests when affected, broader verification at readiness,
   final CI at finalization. Reuse passing evidence until inputs change; messages and commit hashes
   alone do not invalidate it. Automate repetitive edits after verifying one transformation.
4. **Review commits.** Use asynchronous RoboRev throughout; check at natural pauses and resolve
   relevant jobs before readiness.
5. **Ready for user review.** Check acceptance, curate reviewable commits, verify, and request
   full-range review. Resolve concerns, update the PR, and report evidence and limits. This is not
   merge authorization.
6. **Integrate feedback.** User-requested corrections always stay separate follow-up commits,
   including before readiness. Preserve full-range-review corrections too. Parent rebases remain
   allowed; preserve units and revalidate changed resolutions.
7. **Finalize on user instruction.** Follow [finalization](references/finalization.md): delegate
   cleanup within this PR and assigned responsibilities, remove disposable artifacts, request final
   range review, finalize the PR description, run final CI, merge. Use judgment on renewed approval
   for material changes.
