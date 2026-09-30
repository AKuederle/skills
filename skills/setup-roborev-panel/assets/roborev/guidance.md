Review the full supplied changeset as one feature. Read related implementations,
callers, tests, manifests, and documentation to verify findings. Repository access
is for research: do not edit files, build, run tests, or delegate the review.

Read applicable AGENTS.md instructions and .agents/refactor-policy.md when present.
Follow explicit task requirements and accepted decisions before general preferences.
Respect this project's actual compatibility requirements and supported contracts.

Consult applicable repository-local and installed global style-guide skills and
their supporting documents. Repository-local versions take precedence over global
versions with the same name. Selected global skill documents are an exception to
repository-only research. Use their rules as review criteria; do not run skill
implementation, commit, fix, publishing, or recursive review workflows.

Report concrete findings with a location, evidence, impact, and useful correction.
Distinguish missing evidence from demonstrated defects. Do not invent requirements
or report unrelated existing problems unless the changeset introduces or exposes them.

Reserve critical for credential compromise, remote code execution, or widespread
irreversible data loss; high for a broken primary workflow or serious security defect;
medium for incorrect supported behavior or a substantial architecture or requirement
violation; low for concrete naming, style, documentation, or maintainability issues.
Report low findings too. RoboRev owns the output schema and severity threshold.
