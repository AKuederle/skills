# Design it twice

Use when the interface choice is consequential or the first design leaves callers coordinating
internals. Compare materially different designs for the same current requirements.

1. State the supported behavior, constraints, dependencies and ownership requirements.
2. Sketch at least two interfaces with realistic caller examples. Vary the shape, not merely names:
   minimize caller coordination, organize around one state owner, or simplify the common operation.
3. Compare depth, locality, valid states, lifecycle ownership and what callers must understand.
4. Recommend the smallest design that satisfies the requirements. State important tradeoffs.

Independent subagent designs can help when delegation is authorized and useful. Otherwise sketch
alternatives directly. Respect the user's model choices; do not require a particular model or count.

Avoid speculative flexibility. Use [DEEPENING.md](DEEPENING.md) when the choice restructures an
existing cluster, and return to the implementation workflow after choosing the design.
