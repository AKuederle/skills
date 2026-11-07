---
name: handling-pr-reviews
description: Fetch, triage, and address GitHub PR comments and reviews. Complements implementation workflow; does not authorize replies or merge by itself.
---

# Handle PR feedback

Confirm repository/PR, head, and base. Retrieve all conversation comments, reviews, inline comments,
and replies, including pagination. Inspect original revision/context for stale locations; outdated
does not mean resolved.

Track IDs/URLs, reviewed revision, request, disposition, clarifications, and fix commit in existing
task state. Deduplicate; revisit edited comments/new replies. No mandatory plan document.

- User comments are this PR's requirements unless explicitly changed/deferred; do not silently drop
  them.
- Evaluate other reviewers against actual code, supported contracts, requirements, conventions, and
  scope. Reject incorrect or unnecessary suggestions with reasons.
- Inspect artifacts before asking about consequential ambiguity; clarify before dependent edits
  while continuing independent work. Discuss large scope expansions or architectural conflicts;
  routine in-scope fixes need no approval.

Delegate fixes with exclusive ownership, comment/context, clarifications, acceptance criteria, and
focused checks. Parallelize independent work, sequence overlaps/dependencies, respect model
constraints; report unavailable delegation. Workers own assigned corrections, not the lifecycle.
Parent owns tracking, integration, commits and PR delivery unless reassigned.

Use [implement-code-change](../implement-code-change/SKILL.md) for follow-up commits and
proportionate verification; preserve user corrections separately, reuse valid evidence, and do not
restart the lifecycle per comment.

Reconcile every request before claiming completion: verified fix, already satisfied, user-approved
deferral/rejection, reasoned rejection of external feedback, or unresolved. Report pending decisions
and coverage limits. Reply/resolve threads only when authorized, with factual verified disposition.
Checking feedback alone authorizes neither posting nor finalization/merge.
