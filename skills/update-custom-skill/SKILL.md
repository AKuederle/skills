---
name: update-custom-skill
description: Update an installed agent skill at its original source using skills lockfiles, create a PR for user-managed sources, and reinstall the updated version. Ask before copying third-party skills into the personal repository.
metadata:
  source-repository: "https://github.com/AKuederle/skills"
  source-path: "skills/update-custom-skill/SKILL.md"
---

# Update a custom skill

## Resolve the installation and source

Identify the installed skill, its directory, installation scope, and agent targets. Save the target project's absolute path before changing directories. If different global and local copies exist and the request does not identify one, ask which to update.

For a skill installed through `npx skills`, look up its exact name in the matching lockfile:

- Project installation: `skills-lock.json` at the target project root.
- Global installation: `$XDG_STATE_HOME/skills/.skill-lock.json` when configured, otherwise `~/.agents/.skill-lock.json`.

Read `source`, `sourceType`, `sourceUrl`, `skillPath`, `ref`, and `installMode` when present. Use the lock entry to resolve the original repository and skill path, not the installed directory's containing repository. A symlink to the installer's canonical directory is not a link to the authoring source. Lockfile schemas vary; inspect the actual entry rather than assuming every field exists.

If no lock entry exists, inspect the resolved symlink target and `metadata.source-repository` / `metadata.source-path` in `SKILL.md`. A project-authored skill with no external source is edited in its current repository. If the source remains ambiguous, ask instead of inventing provenance. Never edit only an installed copy when its authoring source is elsewhere.

Compare the installed contents with the resolved source revision before reinstalling. Preserve any local edits; reconcile relevant differences into the proposed source change instead of overwriting them.

## Choose source ownership

`AKuederle/skills` and repositories owned by `AKuederle` are user-managed. Treat another repository as user-managed only when the user has identified it as such or there is clear ownership evidence. Having permission to contribute does not establish that it is user-managed.

**User-managed source:** Reuse a matching checkout or clone it. Read repository guidance, preserve unrelated work, and make the requested changes on a feature branch at the original `skillPath`. Create a PR there, including when the source is a project repository rather than the personal skill collection. No extra permission question is needed for this route.

**Third-party source:** Before copying or changing it, ask whether the user wants a customized copy in `https://github.com/AKuederle/skills`. Wait for the answer. If they decline, stop without changing the source or installation. If they agree, copy the full skill and required resources into `skills/<name>/` in that repository. Preserve license notices and attribution, record the upstream repository, path, and revision, then make the requested changes and create a PR in the personal repository. Keep the installed name unless a collision requires a user choice. Do not submit a third-party upstream PR unless separately requested.

For a customized copy, set `metadata.source-repository` and `metadata.source-path` to its new personal-repository source; retain upstream provenance separately, for example `metadata.upstream-repository`, `metadata.upstream-path`, and `metadata.upstream-revision`. For an existing user-managed skill, add or correct source metadata to identify its actual source. Do not put a machine-specific checkout path into distributed provenance.

## Validate, publish, and reinstall every update

Use available system skill-authoring guidance to validate the changed skill and any changed helpers. Check discovery with `npx -y skills@latest add . --list` from the source checkout. Commit, push, and create or update the PR for the requested changes. Do not merge unless asked. Link the PR through the host's PR-linking tool when available.

Immediately reinstall the changed skill from the pushed PR head. Do this after each subsequent revision too; an open PR is not a reason to leave the old version installed. Use the source repository's GitHub URL, exact pushed commit SHA, and skill directory, for example:

```sh
npx -y skills@latest add 'https://github.com/OWNER/REPO/tree/HEAD_SHA/SKILL_DIRECTORY' --skill NAME --global --agent codex --yes
```

Preserve installation scope and existing agent targets. For project installations, run in the saved target project directory and omit `--global`. Preserve copy mode with `--copy` when recorded. Install only the affected skill, not the whole collection. Use an equivalent revision-specific Git source for other supported hosts. Do not install from unchanged `main` or from a temporary checkout path that loses remote provenance.

For a project-authored skill whose source is already its active installation directory, the source edit itself updates the installed skill. Verify it there; do not install a second copy through `npx skills`.

Verify installed files match the updated source, and the appropriate lock entry now points to that repository, skill path, and pushed revision. After copying a third-party skill, verify that the lock points to the personal source. Preserve unrelated entries and report target-project lockfile changes separately. A commit pin is intentional; a later refresh after merge must explicitly select the desired branch or release.

Return the PR link, source location, installation scope and revision, and validation result. If installation fails, report that separately from successful publication and do not claim the update is installed.
