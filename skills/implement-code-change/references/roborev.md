# RoboRev reviews

Check that RoboRev operates in this repository before implementation. If setup is missing or
broken, report the concrete failure and use [setup-repo](../../setup-repo/SKILL.md). Do not silently
skip required review. Panel configuration is optional and manual through
[setup-roborev-panel](../../setup-roborev-panel/SKILL.md); do not configure a panel as part of a feature.

## Per-commit reviews

Let automatic commit reviews run while implementing. Confirm that each commit has a job; enqueue
`roborev review <sha>` when the hook did not. Check at natural pauses:

```bash
roborev list --status running --status queued --status done --open
roborev show --job <job-id> --json
```

Assess findings against the code, requirements, and repository conventions. Review feedback does
not expand scope. Explain rejected findings; fix valid ones using the
[commit rules](commits.md). Comment on and close resolved jobs, including reviews without findings.
Wait for relevant pending jobs at readiness boundaries rather than blocking every implementation slice.

## Full-range reviews

Run at readiness for user review and again during user-triggered finalization. Inspect effective
repository and global configuration: use `feature_ready` when configured, otherwise force normal
RoboRev review with `--panel none`. Diagnose an invalid configured panel or failed reviewer rather
than silently substituting another review.

Use the actual task/PR base, including the parent branch for stacked work. Make accepted requirements
and deviations available in the reviewed checkout or configured includes. Reviewers do not inherit
the conversation. Record the base, head, and returned job ID:

```bash
roborev review --since <stack-base> --panel feature_ready
# Without a configured feature_ready panel:
roborev review --since <stack-base> --panel none
roborev wait --job <job-id>
roborev show --job <job-id> --json
```

Panels need the daemon; `--local` does not fan out. Inspect member statuses and findings. Resolve
accepted corrections and their automatic reviews, comment on dispositions, and close the range
review. Preserve correction commits. Do not repeat a full-range review solely to review its fixes;
use judgment when changes invalidate the broader review. Finalization is a separate review gate.
