---
name: update-custom-skill
description: Update an installed agent skill at its original source using skills lockfiles, create a PR for user-managed sources, and reinstall the updated version. Ask before copying third-party skills into the personal repository.
---

# Update a custom skill

## Resolve the installation and source

Identify the installed skill, its directory, installation scope, and agent targets. Save the target project's absolute path before changing directories. If different global and local copies exist and the request does not identify one, ask which to update.

For a skill installed through `npx skills`, look up its exact name in the matching lockfile:

- Project installation: `skills-lock.json` at the target project root.
- Global installation: `$XDG_STATE_HOME/skills/.skill-lock.json` when configured, otherwise `~/.agents/.skill-lock.json`.

Read `source`, `sourceType`, `sourceUrl`, `skillPath`, `ref`, and `installMode` when present. Use the lock entry to resolve the original repository and skill path, not the installed directory's containing repository. A symlink to the installer's canonical directory is not a link to the authoring source. Lockfile schemas vary; inspect the actual entry rather than assuming every field exists.

If no lock entry exists, inspect the resolved symlink target and any existing source metadata in `SKILL.md`. A project-authored skill with no external source is edited in its current repository. If the source remains ambiguous, ask instead of inventing provenance. Never edit only an installed copy when its authoring source is elsewhere.

## Check upstream before editing

Fetch the current upstream skill and compare it with the installed version before changing source or reinstalling. Use the lock's installed revision as the baseline when recoverable, distinguish local edits from upstream changes, and compare the skill's resources as well as `SKILL.md`. For a tracked branch, check its current head. For a commit or tag pin, check the source repository's current development branch too, while respecting that the pin may be intentional. Do not treat a folder hash as a commit SHA. If the baseline or intended upstream cannot be resolved, explain that uncertainty and ask which version to use.

When upstream has changes missing from the installation, explain the meaningful differences to the user, including conflicts with local customizations or the requested edit. Ask how they want to integrate them: incorporate all upstream changes before the requested edit, incorporate selected changes, or keep the installed baseline and apply only the requested edit. Wait for the answer before editing or reinstalling; an update request alone does not select an integration strategy. Preserve local edits and implement the chosen strategy. Reuse an integration choice already given for these differences rather than asking again. If there are no upstream changes, proceed without this question.

## Choose source ownership

For this workflow, every GitHub repository the user can access is **user-managed**, including public repositories and shared or organization repositories, regardless of ownership or write access. Verify access through the user's authenticated GitHub connection or CLI. Access determines this route, not permission to push directly: use the normal fork-and-PR route if direct branch pushes are unavailable. Do not classify an authentication failure alone as proof that a repository is third-party; resolve missing access information with the user. For non-GitHub sources, use the user's stated management relationship.

**User-managed source:** Reuse a matching checkout or clone it. Read repository guidance, preserve unrelated work, and make the requested changes on a feature branch at the original `skillPath`, using the agreed upstream integration strategy when applicable. Create a PR there, including when the source is a project repository rather than the personal skill collection. No ownership confirmation is needed for this route; the upstream integration question still applies when versions differ.

**Third-party source:** Resolve the user's personal skills repository from their explicit instructions or configuration. The lock entry for an installed custom-skill management skill can identify a candidate; confirm it is the intended personal repository rather than assuming its publisher owns the user's skills. If the destination is unclear, ask which repository to use together with asking whether the user wants a customized copy there. Otherwise, ask whether they want a customized copy in the resolved personal repository before copying or changing it. Wait for the answer. If they decline, stop without changing the source or installation. If they agree, copy the full skill and required resources into `skills/<name>/` in that repository. Preserve license notices and attribution, record the upstream repository, path, and revision, then make the requested changes and create a PR in the personal repository. Keep the installed name unless a collision requires a user choice. Do not submit a third-party upstream PR unless separately requested.

For a customized copy, preserve upstream provenance and attribution with its resources, for example in existing license notices or upstream metadata. The new installation's lock entry must identify the personal source; duplicate source metadata is not required. Correct any existing source metadata that would misleadingly identify the copied skill's authoring source. Do not put a machine-specific checkout path into distributed provenance.

## Validate, publish, and reinstall every update

Use available system skill-authoring guidance to validate the changed skill and any changed helpers. Check discovery with `npx -y skills@latest add . --list` from the source checkout. Commit, push, and create or update the PR for the requested changes. Do not merge unless asked. Link the PR through the host's PR-linking tool when available.

Immediately reinstall the changed skill from the pushed PR head. Do this after each subsequent revision too; an open PR is not a reason to leave the old version installed. Use the source repository's GitHub URL, exact pushed commit SHA, and skill directory, for example:

```sh
npx -y skills@latest add 'https://github.com/OWNER/REPO/tree/HEAD_SHA/SKILL_DIRECTORY' --skill NAME --global --agent codex --yes
```

Preserve installation scope and existing agent targets. For project installations, run in the saved target project directory and omit `--global`. Preserve copy mode with `--copy` when recorded. Install only the affected skill, not the whole collection. Use an equivalent revision-specific Git source for other supported hosts. Do not install from unchanged `main` or from a temporary checkout path that loses remote provenance.

Every custom skill not exclusive to the current project must be installed through `npx skills`. If such a skill was previously copied manually or linked directly to source, migrate that installation to `npx skills` after publishing the update, preserving scope and agent targets. If a fork supplied the PR head, install from that fork's pushed commit and report both the PR's target repository and the source now recorded in the lock.

For a project-authored skill whose source is already its active installation directory, the source edit itself updates the installed skill. Verify it there; do not install a second copy through `npx skills`.

Verify installed files match the updated source, and the appropriate lock entry now points to that repository, skill path, and pushed revision. After copying a third-party skill, verify that the lock points to the personal source. Preserve unrelated entries and report target-project lockfile changes separately. A commit pin is intentional; a later refresh after merge must explicitly select the desired branch or release.

Return the PR link, source location, installation scope and revision, and validation result. If installation fails, report that separately from successful publication and do not claim the update is installed.
