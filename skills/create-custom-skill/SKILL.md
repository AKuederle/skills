---
name: create-custom-skill
description: Create a personal agent skill, choose whether its source belongs in this project or the personal skills repository, and publish and install reusable skills.
metadata:
  source-repository: "https://github.com/AKuederle/skills"
  source-path: "skills/create-custom-skill/SKILL.md"
---

# Create a custom skill

## Choose scope first

Ask the user which scope they want, unless they already chose explicitly:

1. **User-wide:** Useful across projects. Keep source in `AKuederle/skills` and install globally.
2. **This project only:** Only meaningful in the current project. Create `.agents/skills/<name>/SKILL.md` in that project, following its guidance. Validate it, report the path, and stop here. Ignore the rest of this skill; no personal-repository PR or `npx skills` installation is needed.
3. **Reusable for certain projects:** Useful in multiple projects meeting particular requirements. Keep source in `AKuederle/skills`, describe those requirements in the skill's description and instructions, and install only into the current project. Other suitable projects can install it manually.

Keep the target project's absolute path before changing directories. Scope controls both source ownership and installation; a reusable skill installed locally still has its source in the personal repository.

## Author reusable source

The personal repository is `https://github.com/AKuederle/skills`. Reuse a matching local checkout when available, otherwise clone it. Read its repository guidance, preserve unrelated changes, and work on a feature branch.

Create `skills/<name>/SKILL.md` and any needed resources. Use available system skill-authoring guidance for the content and validation. Keep instructions focused on the requested capability. Add provenance in frontmatter, replacing the path with this skill's actual source path:

```yaml
metadata:
  source-repository: "https://github.com/AKuederle/skills"
  source-path: "skills/<name>/SKILL.md"
```

Validate the skill and any executable helpers. Run `npx -y skills@latest add . --list` from the source repository to check discovery. Add its entry to the repository's existing skill catalog.

## Publish and install

Commit the requested source changes, push the feature branch, and create a PR. Creating a PR and installing the requested skill are part of this workflow; do not add another confirmation step. Do not merge the PR unless asked. Link the PR through the host's PR-linking tool when available.

Install from the pushed PR head, not the default branch, so the user gets the new skill immediately. For GitHub sources use the full skill-directory URL, with the pushed commit SHA in place of `HEAD_SHA`:

```sh
npx -y skills@latest add 'https://github.com/AKuederle/skills/tree/HEAD_SHA/skills/NAME' --skill NAME --global --agent codex --yes
```

For reusable project skills, run from the saved target project directory and omit `--global`. Select the user's requested agent, otherwise the current agent. Do not install every skill or target every agent. An intentional commit pin keeps the PR version installed until explicitly refreshed after merge.

Verify the installed files match the source and the matching lock entry records the repository, skill path, and requested revision. Global provenance normally lives in `~/.agents/.skill-lock.json`, or `$XDG_STATE_HOME/skills/.skill-lock.json` when configured; project provenance lives in the target project's `skills-lock.json`. Preserve unrelated lock entries. If installation fails, report the PR and failure without claiming the skill is active.

Return the PR link, installation scope, installed revision, and validation result. Project-local lockfile changes belong to the target project; report them without bundling them into the skills-repository PR.
