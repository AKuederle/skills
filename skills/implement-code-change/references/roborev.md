# RoboRev

Before implementation, check RoboRev works; report broken/missing setup and use
[setup-repo](../../setup-repo/SKILL.md), never silently skip review. Panel setup is optional and
manual through [setup-roborev-panel](../../setup-roborev-panel/SKILL.md).

Automatic hooks create commit review jobs. Implement while reviews run; inspect results at natural
pauses:

```bash
roborev list --status running --status queued --status done --open
roborev show --job <job-id> --json
```

Assess against code, accepted scope, and conventions. Fix valid findings under [commit
rules](commits.md); explain rejected ones. Comment/close resolved jobs, including passes; wait for
pending relevant jobs at readiness.

Request full-range review at user-review readiness and again at user-triggered finalization. Use the
actual task/PR/parent base, not just HEAD. Supply requirements/deviations in the checkout or
configured includes. Record base/head/job. Use effective `feature_ready` if configured, otherwise
`--panel none`; diagnose invalid panels/reviewer failures rather than silently falling back.

```bash
roborev review --since <base> --panel feature_ready
# Or, without that configured panel:
roborev review --since <base> --panel none
roborev wait --job <job-id>
roborev show --job <job-id> --json
```

Inspect member statuses. Resolve findings and
correction-commit reviews, then close jobs. Preserve corrections. Repeat range review when broader
evidence is invalidated, not merely for its fixes; finalization remains a separate gate.
