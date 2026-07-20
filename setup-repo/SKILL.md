---
name: setup-repo
description: Use when preparing a serious code repository for agent-driven implementation, especially before the first implementation task, when the repository has no accepted backwards-compatibility policy, or when roborev is not installed or configured for the repository.
---

# Set Up a Repository

Establish the repository's backwards-compatibility policy with the user before configuring automated review. Do not infer compatibility requirements from repository age, version numbers, or code alone.

## Establish Project State

Ask the user:

1. Is the project experimental, or is it already deployed and used in production? If it is in production, how important is uninterrupted compatibility for its users?
2. Does the work have consumers outside this repository, such as downstream projects, services, packages, plugins, scripts, or third-party users?

Use the answers to select and tailor one of these positions:

- **Experimental with no external consumers:** Absolutely no backwards compatibility is required. Do not create compatibility shims, aliases, deprecation layers, dual paths, or transitional wrappers. Make hard cuts and update every consumer in the repository in the same change without asking again.
- **Experimental with downstream consumers:** Do not assume compatibility. Discuss breaking changes with the user before making them and provide concrete options so the user can decide where compatibility matters. Display every accepted breaking change prominently in user reports, pull-request descriptions, and changelogs.
- **Production with a loose user base:** Internal refactors and internal behavior changes may be made when safe, but public interfaces should remain stable. When a public break is justified, discuss it with the user, provide a clear migration path, and report it prominently in user reports, pull-request descriptions, and changelogs.
- **Important production project:** Backwards compatibility is mandatory. Do not break existing public interfaces, supported behavior, or persisted data formats without an explicit user-approved exception and migration plan.

If the answers do not fit one category cleanly, choose the stricter applicable policy and call out the ambiguity.

## Propose the Policy

Turn the selected position into exactly three short, repository-specific bullets covering:

- The backwards-compatibility guarantee
- How internal refactors and in-repository consumers must be handled
- How public interfaces, downstream consumers, migrations, and breaking-change communication must be handled

Use hard, definitive language, especially at the extremes. Say **"Absolutely no backwards compatibility is required"** rather than **"backwards compatibility is not required"**, or **"Backwards compatibility is mandatory"** when that is the selected policy.

Present the three bullets to the user for review. Stop and wait for explicit acceptance. If the user requests changes, revise all affected bullets and ask again. Do not write policy files, install software, or initialize roborev before acceptance.

## Record the Accepted Policy

After acceptance, create `./.agents/refactor-policy.md` and write the accepted three bullets under a concise heading. Preserve their meaning and strength; do not soften them while converting them into the file.

## Install and Initialize Roborev

Follow the current official [roborev installation guidance](https://www.roborev.io/installation/) for the user's platform. Prefer the documented quick installer unless the environment or user has an established package-management preference.

Current quick-install commands are:

```bash
# macOS / Linux
curl -fsSL https://roborev.io/install.sh | bash

# Windows PowerShell
powershell -ExecutionPolicy ByPass -c "irm https://roborev.io/install.ps1 | iex"
```

Verify the installed CLI:

```bash
roborev version
```

Then initialize the repository:

```bash
roborev init
```

If installation, verification, or initialization fails, stop setup and report the concrete failure. Do not claim that the repository is ready.

## Configure Review Guidance

Create or update `.roborev.toml` in the repository root. Preserve existing settings and ensure there is exactly one top-level `review_guidelines` key. Add or update this section:

```toml
# Project-specific review guidelines
review_guidelines = """
<additional guidance>
"""
```

Replace `<additional guidance>` with a brief reviewer-facing version of the accepted refactor policy. State what roborev must flag or permit; do not paste generic setup prose. Keep the wording consistent with `.agent/refactor-policy.md`, including whether compatibility shims are forbidden, optional by explicit decision, or required.

Validate the resulting configuration and confirm the CLI can operate in the repository:

```bash
roborev config list --local
roborev list --status running --status queued --status done --open
```

No review jobs is a valid result during initial setup. A command or configuration failure is not. Report the accepted policy, files created or changed, roborev version, and verification result when setup completes.
