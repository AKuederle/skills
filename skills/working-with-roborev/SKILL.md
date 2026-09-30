---
name: working-with-roborev
description: Use whenever working in a repository where roborev automatically reviews commits, especially while implementing multi-commit changes, checking asynchronous review feedback, addressing review findings, or completing a feature.
---

# Work With Roborev

Roborev reviews every commit asynchronously. Keep implementing while reviews run, and check them at natural pauses such as while long-running tests execute or after finishing an implementation slice.

Follow the [reviewable-commits skill](../reviewable-commits/SKILL.md) for commit boundaries, messages, active fixups, and final stack curation. This skill adds the Roborev-specific review lifecycle.

When an implementation appears complete, the [finish-implementation-stack skill](../finish-implementation-stack/SKILL.md) coordinates this review cycle with final stack curation, verification, and delivery.

## Require a Working Roborev CLI

Verify at the start of an implementation task that the `roborev` CLI is installed and can operate in the repository. If it is missing, misconfigured, or unable to perform the review workflow, treat that as a hard failure for the implementation task: stop implementation, report the concrete failure to the user, and recommend running the [setup-repo skill](../setup-repo/SKILL.md) or following the official [roborev installation guidance](https://www.roborev.io/installation/).

Do not silently skip roborev, substitute an informal review, or continue implementation without it. Correct an obvious command or argument mistake before declaring failure, but treat a missing expected review job or a broken repository integration as a setup failure.

## Check Reviews Opportunistically

List open review jobs without blocking ongoing work:

```bash
roborev list --status running --status queued --status done --open
```

Inspect a review in structured form:

```bash
roborev show --job <job_id> --json
```

Assess feedback against the actual code, repository conventions, requirements, PRD, and explicit user instructions. Treat roborev as useful review input, not as authority. Overrule feedback that fundamentally contradicts the PRD or explicit user instructions.

## Triage Feedback During Implementation

When a review contains relevant feedback, decide whether it warrants interrupting the current implementation slice:

- If it can wait safely, finish the current slice and address it after the next commit.
- If it should be handled immediately, first commit the current checkpoint when that produces a coherent, stable commit. Otherwise stash the in-progress work, address the feedback, and then restore and continue the interrupted work.

During active implementation, before the final whole-stack review starts, make review-driven
changes in dedicated fixup commits targeting the reviewed commit that introduced the code:

```bash
git add path/to/affected-files
git commit --fixup=<reviewed-commit>
```

Do not amend the reviewed commit directly. Keep the fixup commits separate during active implementation so the reviewer-driven corrections remain visible and recoverable.

Once final review starts, preserve the finalized stack and use normal additional commits for
every correction, including feedback from automatic reviews of those correction commits.
Never autosquash or rebase these corrections into the reviewed stack or one another.

After resolving a review, summarize the resolution and close the job:

```bash
roborev comment --job <job_id> "<summary>"
roborev close <job_id>
```

Close reviews with no relevant feedback as well, using a concise comment when useful.

## Finish the Review Cycle

Do not wait for asynchronous reviews during ordinary implementation. Only after the full requested work is complete, wait for any remaining relevant reviews on all commits and address them.

Wait for the final commit's review with:

```bash
roborev wait --sha HEAD
```

Then list open jobs again, inspect completed feedback, and address relevant findings using the
commit rules for the current phase. Repeat until no pending relevant feedback remains. Before
declaring a feature complete, close every review for its commits, including reviews with no
relevant findings.

Before final review, after all implementation reviews are resolved and closed, use the final-stack
procedure from the [reviewable-commits skill](../reviewable-commits/SKILL.md) to autosquash the active
implementation fixups into their targets:

```bash
git rebase -i --autosquash <stack-base>
```

After final review starts, skip history curation. Retain normal correction commits and run fresh
verification after the last correction.

## Reviewing features/branches

After final stack curation, follow [references/final-review.md](references/final-review.md).
Use the `feature_ready` panel if configured, otherwise a single whole-stack review. Corrections
remain normal additional commits, including follow-up automatic review corrections. Do not
request repeated whole-stack reviews merely to review fixes from the final review.
