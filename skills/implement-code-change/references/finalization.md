# Finalization

Start on the user's finalize/merge instruction, not readiness or ordinary feedback. Recover scope,
outstanding feedback, base/head, reviews, and verification; reuse completed valid gates after
interruption.

1. Delegate [cleanup-codebase](../../cleanup-codebase/SKILL.md) limited to this PR and assigned
   responsibilities. Pass requirements, policy, range, exclusions, and model constraints. Child does
   cleanup/focused checks; parent retains integration/PR/merge. If delegation is unavailable,
   explain and do the same scoped cleanup directly.
2. Remove this PR's disposable plans/review logs/inventories. Preserve enduring docs, ADRs,
   required/user-owned records; retain necessary decisions and reviewer-accessible requirements
   elsewhere before deletion.
3. Commit cleanup/removals as follow-ups, resolve automatic reviews, and request final full-range
   panel or normal RoboRev using the [review procedure](roborev.md). Resolve concerns and verify
   consumers.
4. Judge whether changes materially alter approval; summarize and obtain renewed approval when
   warranted. Otherwise use the existing merge instruction.
5. Describe the actual final changes precisely in the PR description. Keep it concise; omit PR
   history and routine process narration such as formatting/testing steps. Include evidence that
   explains impact, such as UI screenshots, relevant benchmarks, or test findings that substantiate
   behavior changes. Run required final CI/signoff after
   reviews/revisions are resolved; reuse valid checks where allowed. Fix failures and revalidate
   evidence, confirm PR base/head, then merge/report only with no unresolved checks/reviews.

Preserve feedback/review correction commits; parent rebases are allowed under the [commit
rules](commits.md). Report coverage limits honestly; combined partial checks are not a single full
green run. Leave the cleanest version of this change without future baggage.
