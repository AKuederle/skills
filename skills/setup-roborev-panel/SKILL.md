---
name: setup-roborev-panel
description: Configure a repository's manually triggered RoboRev feature_ready panel and reviewer rubrics when explicitly invoked. Preserves existing settings and does not run reviews.
---

# Set up a RoboRev panel

Configure the target repository's `feature_ready` panel from the bundled
[configuration](assets/feature-ready.toml) and [rubric assets](assets/roborev/).
Use project-local configuration unless the user requests a global setup.

## Inspect and configure

Read the repository's instructions, existing `.roborev.toml`, and any configured
rubrics. Inspect relevant global RoboRev settings for inherited panels and Codex skill
access. Preserve unrelated settings, review guidance, hooks, and user customizations.

Check `roborev version` and the available configuration schema. The assets were tested
with v0.69.0; they require custom review types, includes, and subagent panels. Codex must
support `exec --output-schema` and have access to the configured models. If RoboRev is
missing or too old, report the prerequisite; installing or upgrading the tool and
initializing automatic commit hooks require a request that includes those changes.

Merge the asset tables into `.roborev.toml` and copy the five Markdown assets to
`.agents/roborev/`. Reuse equivalent existing definitions and adapt existing rubrics
instead of overwriting custom criteria. If an existing `feature_ready` panel serves a
different purpose, resolve that conflict with the user before replacing it. Repository
entries replace complete global entries with the same name; include all required keys
when overriding one. Keep top-level settings above TOML table headers.

Use these defaults unless the user specifies alternatives:

- `full_stack`, `plan_conformance`, and `simplicity`: Codex, `gpt-6.1-sol`, high reasoning.
- `conventions`: Codex, `gpt-6-luna`, high reasoning.
- `feature_ready`: all four required members; Codex synthesis with `gpt-6.1-sol`.
- `fix_reasoning = "high"`: supplies synthesis reasoning in v0.69.0 and also affects
  ordinary fix jobs. Disclose this effect when reporting the configuration.

The assets do not set `review.default_panel` or `review.hook_review_panel`. Preserve
existing selectors and report any inherited setting that would automatically select
a panel; the requested `feature_ready` panel is intended for explicit final review.

Adapt rubric references to the repository's actual domain, accepted compatibility
policy, and style guides. Do not assume there are no external consumers. Preserve the
shared guidance for researching surrounding code and installed style skills without
editing, testing, delegating, or invoking implementation and recursive review workflows.
Keep accepted plans and deviations in the reviewed checkout, or add an explicit
configured include. Reviewers do not inherit the implementation session's conversation.

## Global settings when requested

Configuring a local panel does not authorize global configuration changes. If the user
also requests global style-skill access, set
`agent.codex.disable_review_skills = false` globally and append the style-skill guidance
from the shared rubric to existing `review_guidelines`; preserve its current contents.
Each reviewer machine must already have those skills installed. Skipping the personal
Codex config with `ignore_review_user_config = true` can remain enabled.

If global automatic-review defaults are requested, use Codex, `gpt-6.1-sol`, and high
reasoning. For a requested global panel, merge into `~/.roborev/config.toml` and use
stable absolute or home-relative rubric paths, or explain that repository-relative
paths must exist in each target repository. Validate with `--global` instead of `--local`.

## Validate and hand off

Run `roborev config validate --local` from the target repository. Confirm every template
and include exists and preserves `{{ index .Includes "guidance" }}`. Inspect the diff
for overwritten customizations. Report the changed files, validation result, relevant
inherited settings, and any missing prerequisites.

Configuration does not start a review, create commits, push, or install workflow skills.
Do not run an implementation or finishing workflow merely to install the panel.

Explain the manual command, using the feature's actual base:

```bash
roborev review --branch --base <feature-base> --panel feature_ready --wait
```

Alternatively pin the finalized stack base with
`roborev review --since <stack-base> --panel feature_ready`. Panels use the daemon;
`--local` does not fan out. Existing global implementation and finishing skills own the
final-review lifecycle: curate the stack before review, record its head and review job,
and retain accepted corrections as normal additional commits without amending,
fixup/squash, or rebasing them into the reviewed history. This also applies to automatic
reviews of correction commits. Installing the panel does not install that workflow;
report if the target agent's global skills do not contain it rather than inserting
implementation instructions into project guides or rubrics.
