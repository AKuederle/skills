# Personal Codex skills

A collection of installable agent skills for implementation, reviewable Git history, refactoring, repository setup, automated review, test-driven development, and codebase design.

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
npx skills add 'AKuederle/skills#v0.1.0' --skill '*' --global --agent codex --yes
```

Installations that follow the default branch can be updated with:

```bash
npx skills update --global
```

For reproducible installations, pin a release tag and explicitly install the next tag when upgrading. `npx skills install` is currently an alias for `npx skills add`; `add` is used here because it is the primary documented command.

## Skills

### `implement-code-change`

The [`implement-code-change`](skills/implement-code-change/) skill is the main entry point for implementation work in serious repositories. It coordinates reviewable commits and roborev, applies the repository's refactoring policy, routes behavioral work to TDD, routes structural work to the structural workflow, and handles mixed changes as separate slices.

### `reviewable-commits`

The [`reviewable-commits`](skills/reviewable-commits/) skill treats active implementation commits and final stack curation as one workflow. It gates every completed slice on verification and commit, preserves follow-ups as targeted fixup or squash commits, and requires a coherent bottom-up review stack before completion.

### `setup-repo`

The [`setup-repo`](skills/setup-repo/) skill establishes an explicit backwards-compatibility policy with the user, records it in `.agents/refactor-policy.md`, installs and initializes roborev, and encodes the policy in repository-specific review guidance.

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

## Repository layout

Each skill lives at `skills/<name>/SKILL.md`, the standard collection layout discovered by the `skills` CLI. Supporting references and agent-specific interface metadata remain inside their corresponding skill directories.

Validate local discovery before publishing a change:

```bash
npx -y skills@latest add . --list
```

## Provenance and license

The original TDD skill was installed with the `skills` CLI from `skills/engineering/tdd/SKILL.md`; that provenance is recorded in `~/.agents/.skill-lock.json` under the `tdd` entry.

Original material in this repository is available under the [MIT License](LICENSE). Material copied from Matt Pocock's repository remains subject to its [upstream MIT license](UPSTREAM_LICENSE.md).
