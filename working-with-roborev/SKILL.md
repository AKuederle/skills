---
name: working-with-roborev
description: Use whenever working in a repository where roborev automatically reviews commits, especially while implementing multi-commit changes, checking asynchronous review feedback, addressing review findings, or completing a feature.
---

# Work With Roborev

Roborev reviews every commit asynchronously. Keep implementing while reviews run, and check them at natural pauses such as while long-running tests execute or after finishing an implementation slice.

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

Make every set of changes prompted by a roborev review in a dedicated commit. Do not amend an earlier commit or fold the review changes into it. This keeps the reviewer-driven correction visible in the stack.

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

Then list open jobs again, inspect completed feedback, address relevant findings in dedicated commits, and repeat until no pending relevant feedback remains. Before declaring a feature complete, ensure every roborev review for its commits is closed, including reviews that produced no relevant feedback.

## Reviewing features/branches

At the end of a full implementation stack, you should ask roborev to review the full stack as a whole again:

```bash
roborev review --since abc123       # Review commits since abc123 (exclusive)
```

Don't ask for repeated full stack reviews when addressing findings.
