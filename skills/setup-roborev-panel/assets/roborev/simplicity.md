# Simplicity and necessity review

{{ index .Includes "guidance" }}

Search the library for existing capabilities before recommending new abstractions.
Question redundant wrappers, state, adapters, options, dependencies, and speculative
generality. Identify code that can be removed or replaced with a simpler existing API.
Check compatibility machinery against the project's actual policy.
For each finding, explain the viable simpler approach and the contracts it preserves.
Preserve required security, ownership, lifetime, and supported behavior guarantees.
