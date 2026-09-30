# Final changeset review

Run this gate after finalizing the implementation commit stack. On resumption, reuse the
recorded base, finalized head, and review job; do not curate or enqueue the same review again.

## Select the review and its context

Inspect the effective global and repository RoboRev configuration for a panel named
`feature_ready`. Repository definitions override global definitions with the same name.
Use that panel when configured. A missing optional panel permits a single whole-stack
review; an invalid configured panel or failed reviewer requires diagnosis, not a silent fallback.

Use the exact implementation stack base or PR base. For stacked PRs, use the parent PR's
branch or the recorded parent tip, not an assumed `main`. Do not review only the final commit.

Make the accepted task, plan, PRD, acceptance criteria, and approved deviations available
inside the reviewed checkout or through configured rubric includes. If they only exist in
the conversation or an external issue/PR, record the relevant requirements in the existing
task artifact before finalizing the stack. Do not assume reviewers share the coding session's
conversation or can retrieve external artifacts.

Record the exact base and finalized head in persistent task state before enqueueing:

```bash
roborev review --since <stack-base> --panel feature_ready
```

If `feature_ready` is not configured, force a single review so an unrelated default panel
does not silently replace it:

```bash
roborev review --since <stack-base> --panel none
```

Record the returned parent job ID, wait for that job, and inspect its findings and member
statuses. Panels require the daemon; do not use `--local`. Wait by recorded job ID rather
than `HEAD`, since later correction commits have their own automatic reviews.

## Preserve review-driven corrections

Starting final review freezes the curated stack. Assess findings against the accepted task
and repository conventions. Explain rejected or unavailable-context findings in the review
comment instead of implementing unsupported requirements.

Commit valid corrections as normal additional review units. Explain the problem and correction
in the message and record the originating review job in task state. Do not use `--fixup`,
`--squash`, amend, or a rebase to fold them into the reviewed stack or into other correction
commits. Apply the same rule to follow-up reviews of the correction commits.

Comment on and close the parent review after resolving findings. Resolve automatic reviews of
the additional commits, run fresh relevant verification, and retain every correction commit.
Do not request another full panel merely to review these fixes. Report the original reviewed
head, final review job, and additional commits so the user can inspect the review's effect.
