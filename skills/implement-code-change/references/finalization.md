# User-triggered finalization

Start only when the user asks to finalize and merge. A readiness report or ordinary feedback is
not a merge instruction. Recover the accepted scope, outstanding feedback, current base/head,
review jobs, and verification evidence before continuing.

1. **Delegate cleanup.** Ask a subagent to apply
   [cleanup-codebase](../../cleanup-codebase/SKILL.md), limited to work done in this PR and the
   agent's assigned responsibilities. Aim for the cleanest version of this change, without
   obsolete paths, scaffolding, or other baggage for future maintainers. Pass the requirements,
   compatibility policy, diff range, exclusions, and model constraints. The child owns scoped
   cleanup and focused checks; retain integration, PR, and merge ownership. If delegation is
   unavailable, explain the limitation and perform the same scoped cleanup directly.
2. **Remove disposable artifacts.** Remove this PR's task-only plans, inventories, and review logs. Preserve
   enduring documentation, ADRs, required records, and user-owned artifacts. Transfer still-needed
   decisions and review context to maintained docs, PR context, or task state before deletion.
   Ensure the final reviewers can access the accepted requirements.
3. **Request final review.** Commit cleanup and artifact removal as reviewable follow-ups. Resolve
   their automatic reviews, then request the final full-range panel or normal RoboRev review
   using [the review procedure](roborev.md). Resolve concerns and verify affected consumers.
4. **Assess readiness to merge.** Use judgment about whether cleanup or findings materially change
   what the user approved. If renewed approval is warranted, summarize the concrete changes and
   obtain it before final CI and merge. Otherwise proceed under the existing merge instruction.
5. **Finalize and merge.** Update the PR description to the final outcome, decisions, and evidence.
   Run the required final CI/signoff after reviews and requested revisions are resolved, reusing
   valid checks where allowed. Fix failures and revalidate affected evidence; do not merge with
   unresolved checks or reviews. Confirm the intended PR head/base, merge, and report the result.

Preserve user-feedback and review-correction commits throughout; parent rebases remain allowed
under [the commit rules](commits.md). Do not restart completed gates after a status question or
compaction. Report actual coverage and limits, rather than describing combined partial evidence
as a single green full run.
