# Personal Codex skills

A collection of installable agent skills for implementation, reviewable Git history, refactoring, repository setup, automated review, test-driven development, codebase design, and clear writing.

## Distribution model

This collection is distributed directly from its Git repository. It is not published as an npm skill package and is not intended for submission to skills.sh or an official skill catalog. The Vercel `skills` CLI is used only to discover and install the `SKILL.md` directories from Git.

## Quick start

The collection can be installed directly from this Git repository with the Vercel [`skills`](https://github.com/vercel-labs/skills) CLI.

Inspect the available skills:

```bash
npx skills add AKuederle/skills --list
```

The equivalent explicit Git URL is:

```bash
npx skills add https://github.com/AKuederle/skills.git --list
```

Install the complete suite globally for Codex:

```bash
npx skills add AKuederle/skills --skill '*' --global --agent codex --yes
```

Installing the complete suite is recommended because `implement-code-change` routes work through several companion skills. Installing only the router leaves those relative references unresolved.

After installation, run `$setup-repo` in each serious repository before the first implementation task. It establishes the repository's backwards-compatibility policy, records it in `.agents/refactor-policy.md`, installs and initializes roborev, and adds project-specific review guidance.

## Project-local installation

Omit `--global` to install the suite into the current project and create a `skills-lock.json` that the project can commit:

```bash
npx skills add AKuederle/skills --skill '*' --agent codex --yes
```

Project-local installation is useful for testing a new skill version in one repository without changing the globally installed copy.

## Versioned installation

Releases use repository-wide semantic-version tags because the implementation skills depend on one another. Install an immutable release with:

```bash
npx skills add 'AKuederle/skills#v0.7.0' --skill '*' --global --agent codex --yes
```

Installations that follow the default branch can be updated with:

```bash
npx skills update --global
```

For reproducible installations, pin a release tag and explicitly install the next tag when upgrading. `npx skills install` is currently an alias for `npx skills add`; `add` is used here because it is the primary documented command.

## RoboRev review setup

This setup combines automatic reviews of individual commits with a manually triggered `feature_ready` panel for a complete feature or PR. It was tested with RoboRev v0.69.0. Use that version or newer, a Codex CLI that supports `codex exec --output-schema`, and access to the models below. Install the skills above, then initialize RoboRev in each target repository with `$setup-repo` or `roborev init`.

### Global defaults and access to style skills

Set the automatic reviewer to GPT-6.1 SOL with high reasoning and allow Codex reviewers to discover installed skills:

```bash
roborev config set --global review_agent codex
roborev config set --global review_model gpt-6.1-sol
roborev config set --global review_reasoning high
roborev config set --global agent.codex.disable_review_skills false
```

These settings live in `~/.roborev/config.toml`; repository overrides still take precedence. The older reasoning preset `thorough` also maps to high. `ignore_review_user_config = true` can remain enabled: skipping the personal Codex configuration does not disable discovery of installed global skills.

Append the following to the existing `review_guidelines` string in the global configuration. Preserve any existing guidance; setting `review_guidelines` through the CLI replaces the entire value.

```text
Consult applicable repository-local and installed global style-guide skills and
their supporting documents as review criteria. Repository-local versions take
precedence over global versions with the same name. Selected global skill
documents are an explicit exception to checkout-only file reading. Perform the
review yourself; do not run skill implementation, commit, fix, publishing, or
recursive review workflows.
```

Include the same rule in project review guidance where a project supplies its own `review_guidelines`. Each machine running reviews needs the relevant skills installed; this flag does not install or copy them.

### Configure the feature panel in a repository

Invoke the manually selected [`setup-roborev-panel`](skills/setup-roborev-panel/) skill in the target repository:

```text
$setup-roborev-panel Configure this repository's feature_ready panel.
```

The skill merges the panel into the local configuration, preserves existing guidance and customizations, installs the rubrics, and validates the result. It does not start a review or change global settings. It is available only through explicit invocation.

For manual setup, merge [the complete panel configuration](skills/setup-roborev-panel/assets/feature-ready.toml) into `.roborev.toml`. Preserve existing settings; keep the top-level `fix_reasoning` setting before any TOML table headers. Panel synthesis uses the fix workflow's reasoning setting in v0.69.0, so `fix_reasoning = "high"` also sets reasoning for normal fix jobs.

All four reviewers are required:

| Member | Focus | Model | Reasoning |
| --- | --- | --- | --- |
| `full_stack` | Correctness across application layers | `gpt-6.1-sol` | High |
| `plan_conformance` | Accepted plan and requirements | `gpt-6.1-sol` | High |
| `conventions` | Documented styles and concrete nitpicks | `gpt-6-luna` | High |
| `simplicity` | Necessity, reuse, and simpler approaches | `gpt-6.1-sol` | High |
| Synthesis | Combine member findings | `gpt-6.1-sol` | High via `fix_reasoning` |

Leave `review.default_panel` and `review.hook_review_panel` unset for an explicitly triggered panel. Check inherited global selectors too. Automatic commit reviews continue to use the single reviewer.

Panels can also be defined globally. Named project entries override matching global entries; repeat the complete entry when overriding one. Global rubric paths can be absolute, home-relative, or repository-relative. Repository-relative paths must exist in every reviewed repository. Keeping this example and its rubrics in each project makes project-specific changes reviewable. See RoboRev's [panel configuration](https://www.roborev.io/docs/advanced/subagent-review-panels/) and [custom review types](https://www.roborev.io/docs/advanced/custom-review-types/) for other options.

### Install and adapt the reviewer rubrics

A rubric template is a file containing the criteria for one reviewer. The shared include provides common rules; RoboRev adds the changeset and manages the output format. The skill's assets are the canonical examples:

| Target file | Template |
| --- | --- |
| `.agents/roborev/guidance.md` | [Shared review guidance](skills/setup-roborev-panel/assets/roborev/guidance.md) |
| `.agents/roborev/full-stack.md` | [Full-stack review](skills/setup-roborev-panel/assets/roborev/full-stack.md) |
| `.agents/roborev/plan-conformance.md` | [Plan conformance](skills/setup-roborev-panel/assets/roborev/plan-conformance.md) |
| `.agents/roborev/conventions.md` | [Conventions and nitpicks](skills/setup-roborev-panel/assets/roborev/conventions.md) |
| `.agents/roborev/simplicity.md` | [Simplicity and necessity](skills/setup-roborev-panel/assets/roborev/simplicity.md) |

Copy the five templates from an installed skill or this source checkout:

```bash
# Point this at the installed skill or skills/setup-roborev-panel in a source checkout.
panel_skill_dir=/path/to/setup-roborev-panel
mkdir -p .agents/roborev
cp "$panel_skill_dir"/assets/roborev/*.md .agents/roborev/
```

Inspect existing files before copying over them. Adapt domain rules, guide paths, and compatibility requirements to the project. Commit `.roborev.toml` and these five files in the target repository. Put the accepted plan and approved deviations inside the reviewed checkout, or supply them through additional configured includes. Reviewers can research surrounding code and selected global skill documents, but do not inherit the coding session's conversation. Do not assume they can retrieve an external issue or PR description.

### Run the final changeset review

Validate the configuration, then review the complete feature against its actual base:

```bash
roborev config validate
roborev review --branch --base main --panel feature_ready --wait
```

Replace `main` with the PR's actual base. For a stacked PR, use its parent branch or the recorded stack base. To pin an exact base and keep the job ID for later inspection:

```bash
roborev review --since <stack-base> --panel feature_ready
roborev wait --job <parent-job-id>
roborev show --job <parent-job-id> --json
```

Panels require the daemon; `--local` does not fan out. Inspect member statuses as well as the combined review. Worker capacity controls concurrency; optionally set global `max_workers` to at least four for this panel.

The updated [`implement-code-change`](skills/implement-code-change/), [`finish-implementation-stack`](skills/finish-implementation-stack/), and [`working-with-roborev`](skills/working-with-roborev/) skills handle this final gate:

1. Resolve automatic commit reviews, finish verification, and curate the implementation stack.
2. Record the exact base and finalized head. Invoke `feature_ready` when configured; otherwise run one whole-stack review with `--panel none`. An invalid panel or failed reviewer needs diagnosis.
3. Once final review starts, preserve the curated stack. Commit accepted corrections as normal additional commits. Do not amend, use fixup/squash commits, or rebase corrections into the reviewed history. This also applies to subsequent automatic reviews of correction commits.
4. Explain rejected findings, comment on and close the parent review, resolve automatic reviews of correction commits, and run fresh relevant verification. Do not repeat the full panel just to review corrections.
5. Report the reviewed head, parent job ID, and correction commits so the user can assess the panel's effect.

Use a version of the installed skills containing this final-review workflow; updating RoboRev alone does not update skills. The v0.7.0 installation example above predates this gate. Until a release containing it is available, install from a source checkout containing these changes. From that checkout's root, run:

```bash
pnpm dlx skills add . --skill '*' --global --agent codex --yes
```

The complete gate is documented in [the final-review reference](skills/working-with-roborev/references/final-review.md).

## Skills

### `implement-code-change`

The [`implement-code-change`](skills/implement-code-change/) skill is the main entry point for implementation work in serious repositories. It coordinates reviewable commits and roborev, applies the repository's refactoring policy, routes behavioral work to TDD, routes structural work to the structural workflow, and handles mixed changes as separate slices.

### `reviewable-commits`

The [`reviewable-commits`](skills/reviewable-commits/) skill treats active implementation commits and final stack curation as one workflow. It gates every completed slice on verification and commit, uses targeted fixup or squash commits during implementation, and requires a coherent bottom-up review stack. Once final review starts, corrections remain normal additional commits.

### `finish-implementation-stack`

The [`finish-implementation-stack`](skills/finish-implementation-stack/) skill is the required final phase for implementation work. It rechecks acceptance criteria, closes automatic RoboRev reviews, curates and verifies the commit stack, runs the final changeset review with `feature_ready` when configured, preserves correction commits, and ensures fresh verification and delivery before completion.

### `setup-repo`

The [`setup-repo`](skills/setup-repo/) skill establishes an explicit backwards-compatibility policy with the user, records it in `.agents/refactor-policy.md`, installs and initializes roborev, and encodes the policy in repository-specific review guidance.

### `setup-roborev-panel`

The explicitly invoked [`setup-roborev-panel`](skills/setup-roborev-panel/) skill installs the local `feature_ready` panel and its reusable rubrics, preserving repository settings. Its bundled configuration and templates also support manual setup. Installation does not run reviews or change global configuration.

### `working-with-roborev`

The [`working-with-roborev`](skills/working-with-roborev/) skill coordinates asynchronous roborev feedback throughout implementation. It explains when to inspect or defer feedback, requires dedicated commits for review-driven changes, and closes every review before a feature is considered complete.

### `backwards-compatibility`

The [`backwards-compatibility`](skills/backwards-compatibility/) skill is loaded by refactoring workflows rather than invoked directly. It applies the repository's accepted compatibility policy and requires refactors to improve local understanding and fully replace the superseded design.

### `structural-code-change`

The [`structural-code-change`](skills/structural-code-change/) skill handles behavior-preserving refactoring and mechanical changes under a green baseline. It rejects tests that merely assert names, paths, deleted source text, or other implementation details.

### `tdd`

The [`tdd`](skills/tdd/) directory is a faithful copy of the upstream [TDD skill from mattpocock/skills](https://github.com/mattpocock/skills/tree/main/skills/engineering/tdd), including its test-style, mocking, and OpenAI interface files. The copy was checked against upstream `main` on 2026-07-20.

### `codebase-design`

The [`codebase-design`](skills/codebase-design/) directory is a faithful copy of the upstream [codebase-design skill from mattpocock/skills](https://github.com/mattpocock/skills/tree/main/skills/engineering/codebase-design). It provides shared vocabulary and guidance for deep modules, seam placement, testable interfaces, deepening shallow module clusters, and comparing alternative designs. The copy was checked against upstream `main` at commit `9603c1cc8118d08bc1b3bf34cf714f62178dea3b` on 2026-07-20.

### `unslop`

The [`unslop`](skills/unslop/) directory vendors the upstream [unslop skill from cursor/plugins](https://github.com/cursor/plugins/tree/main/pstack/skills/unslop). It removes common AI-writing patterns while retaining the writer's intended meaning and voice. This copy adds one sentence that uses ASD-STE100 Simplified Technical English (STE) as the basis for clear, direct technical writing. It was checked against upstream `main` at commit `60c641e4fad674784b30abcf9f8915dea39df38d` on 2026-08-19.

### `reference-dependency-checkouts`

The [`reference-dependency-checkouts`](skills/reference-dependency-checkouts/) skill helps agents investigate complicated third-party dependencies from a reusable local source checkout. It reuses or safely updates matching clones under `/home/arne/Documents/repos/_references/` before creating a new one, regardless of the dependency's programming language.

## Repository layout

Each skill lives at `skills/<name>/SKILL.md`, the standard collection layout discovered by the `skills` CLI. Supporting references and agent-specific interface metadata remain inside their corresponding skill directories.

Validate local discovery before publishing a change:

```bash
npx -y skills@latest add . --list
```

## Provenance and license

The original TDD skill was installed with the `skills` CLI from `skills/engineering/tdd/SKILL.md`; that provenance is recorded in `~/.agents/.skill-lock.json` under the `tdd` entry.

Original material in this repository is available under the [MIT License](LICENSE). Material copied from Matt Pocock's repository remains subject to its [upstream MIT license](UPSTREAM_LICENSE.md). The vendored `unslop` skill remains subject to its [upstream MIT license](skills/unslop/LICENSE).
