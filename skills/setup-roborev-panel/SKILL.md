---
name: setup-roborev-panel
description: Manually configure the optional feature_ready RoboRev panel and rubrics when explicitly invoked. Preserve settings; do not run reviews.
---

# Set up a RoboRev panel

Use bundled [configuration](assets/feature-ready.toml) and five [rubric assets](assets/roborev/).
Default to project-local configuration; global changes require an explicit request.

Inspect repository instructions, local/global config, inherited selectors, and existing rubrics.
Preserve unrelated settings, hooks, guidance, and customizations; reuse/adapt equivalent
definitions. Resolve conflicting feature_ready purpose with the user. Named local entries replace
complete global entries: include all keys. Keep top-level settings before TOML tables.

Check RoboRev/schema (assets tested v0.69.0), custom types/includes/panels,
`codex exec --output-schema`, and model access. Report missing prerequisites; setup alone does not authorize tool
install/upgrade or hook initialization.

Merge config and copy/adapt rubrics to `.agents/roborev/`. Defaults:
full_stack/plan_conformance/simplicity = Codex gpt-6.1-sol high; conventions = Codex gpt-6-luna
high; all four required; synthesis = Codex gpt-6.1-sol with `fix_reasoning=high`. This sets synthesis
reasoning and also affects ordinary fix jobs; disclose that effect. Preserve selectors; report
inherited automatic-panel selection rather than silently changing it.

Adapt domain, policy, and guide references. Keep reviewer research/style-skill access without
implementation, editing/testing/delegation, or recursive review workflows. Requirements/deviations
must be in the checkout or configured includes, not assumed conversation access.

Only when requested: global style access sets agent.codex.disable_review_skills=false and appends
shared guidance to existing review_guidelines; skills must already exist on reviewer machines;
ignore_review_user_config=true may remain. Requested global reviewer defaults:
Codex/gpt-6.1-sol/high. Global panels need stable absolute/home rubric paths or per-repo paths
explicitly present everywhere.

Run `roborev config validate --local` (or `--global` for global setup); check existing
templates/includes and shared `{{ index .Includes "guidance" }}`. Inspect diff; report changes,
validation, inheritance, prerequisites, and side effects. No review, commit, push, workflow
installation, or implementation lifecycle.

Explain manual full-range review against the actual feature/parent base:

```bash
roborev review --branch --base <base> --panel feature_ready --wait
# Or pin the base:
roborev review --since <base> --panel feature_ready
```

Panels use the daemon, not `--local`. [implement-code-change](../implement-code-change/SKILL.md) owns delivery,
preserved correction commits, and permitted parent rebases; report a missing workflow rather than
inserting it into project guides/rubrics.
