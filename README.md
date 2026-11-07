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
npx skills add 'AKuederle/skills#v0.8.0' --skill '*' --global --agent codex --yes
```

Installations that follow the default branch can be updated with:

```bash
npx skills update --global
```

For reproducible installations, pin a release tag and explicitly install the next tag when upgrading. `npx skills install` is currently an alias for `npx skills add`; `add` is used here because it is the primary documented command.

## RoboRev review setup

`setup-repo` initializes automatic reviews of individual commits. The optional feature panel is
configured separately, only when the user invokes
[`setup-roborev-panel`](skills/setup-roborev-panel/):

```text
$setup-roborev-panel Configure this repository's feature_ready panel.
```

That skill owns the setup instructions, [configuration](skills/setup-roborev-panel/assets/feature-ready.toml),
and [reviewer rubrics](skills/setup-roborev-panel/assets/roborev/). It preserves existing settings,
validates configuration, and does not run reviews. It also covers requested global settings and
reviewer access to installed style skills. The assets were tested with RoboRev v0.69.0.

The implementation workflow requests full-range review when work is ready for user review and
again during user-triggered finalization. It uses the configured `feature_ready` panel, or normal
RoboRev with `--panel none` when that panel is absent. It never configures a panel automatically.
See [the review reference](skills/implement-code-change/references/roborev.md) for commands and context.

## Skills

### `implement-code-change`

[`implement-code-change`](skills/implement-code-change/) is the lifecycle router: align, implement
and commit, verify proportionately, review each commit, prepare for user review, integrate feedback,
and finalize only on the user's merge instruction. Its references own commit, RoboRev, refactor-policy,
and finalization mechanics. User-feedback corrections stay as follow-up commits; parent rebases remain
allowed. The agent judges whether cleanup warrants renewed merge approval.

Subagents apply the guidance within their assignment and return a focused handoff. The parent owns
the main workflow unless it explicitly delegates those responsibilities.

It routes design to `codebase-design`, behavioral testing to `tdd`, mechanical changes to
`structural-code-change`, and scoped final cleanup to `cleanup-codebase`.

The former `reviewable-commits`, `working-with-roborev`, `finish-implementation-stack`, and
`backwards-compatibility` skills are consolidated into this router's references. When upgrading,
remove those retired installed skills so their older workflows do not remain active.

### `handling-pr-reviews`

[`handling-pr-reviews`](skills/handling-pr-reviews/) retrieves complete GitHub feedback, tracks
requests and dispositions, and delegates scoped corrections within the parent workflow. It adapts
the ONE-local skill without command recipes, mandatory plan templates, or fixed test reruns.
Replace older repository-local copies after installing the shared version so they no longer shadow it.

### `cleanup-codebase`

[`cleanup-codebase`](skills/cleanup-codebase/) finds common causes of structural problems and
removes obsolete code, comments, tests, and temporary machinery. During finalization it stays within
this PR and the agent's assigned responsibilities. A delegated cleanup returns focused evidence to
the integration owner rather than running another delivery workflow.

### `setup-repo`

[`setup-repo`](skills/setup-repo/) establishes an explicit backwards-compatibility policy with the
user, records it in `.agents/refactor-policy.md`, initializes RoboRev, and adds project review guidance.

### `setup-roborev-panel`

The explicitly invoked [`setup-roborev-panel`](skills/setup-roborev-panel/) configures the optional
`feature_ready` panel and its rubrics, preserving existing settings. Setup does not run reviews.

### `structural-code-change`

The [`structural-code-change`](skills/structural-code-change/) skill handles behavior-preserving refactoring and mechanical changes under a green baseline. It rejects tests that merely assert names, paths, deleted source text, or other implementation details.

### `tdd`

The [`tdd`](skills/tdd/) directory adapts the upstream [TDD skill from mattpocock/skills](https://github.com/mattpocock/skills/tree/main/skills/engineering/tdd), originally copied on 2026-07-20. It uses concise behavioral-test guidance, caller-visible behavioral increments, and one [examples reference](skills/tdd/tests.md), with static verification for mechanical edits.

### `codebase-design`

The [`codebase-design`](skills/codebase-design/) directory adapts the upstream [codebase-design skill from mattpocock/skills](https://github.com/mattpocock/skills/tree/main/skills/engineering/codebase-design), originally copied at `9603c1cc8118d08bc1b3bf34cf714f62178dea3b` on 2026-07-20. It owns the design vision, principles and techniques, with concise references for deepening modules and comparing interfaces. Implementation workflow and test quality remain in their companion skills.

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
