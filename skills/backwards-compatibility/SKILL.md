---
name: backwards-compatibility
description: Use only when another skill explicitly references this skill while refactoring, replacing, or modifying existing code or interfaces. Do not invoke it directly.
---

# Apply the repository's refactoring policy

Read `.agents/refactor-policy.md`; stop for `setup-repo` if missing. Follow it, allowing only
explicit, change-scoped user overrides. Improve local reasoning, not line counts. Complete
consumer migrations and remove obsolete references and paths unless compatibility requires them.
Verify coherence beyond passing tests; document retained compatibility. Discuss ineffective
refactors before continuing.
