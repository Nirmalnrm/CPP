# Algorithm Research — Navigation on Unknown Competition Maps

**Status:** Index and Week 4 preliminary strategy (not experimental results).

The final Capture The Flag map is not given to the agent before the match. The agent must observe local cells, choose where to explore, find objectives and react to another team's agent. A single shortest-path algorithm cannot discover a goal that has not been observed.

## Proposed strategy

1. Maintain host-compatible safe local movement and validate action outcomes.
2. Prefer informative novel areas; use frontier exploration only when map/pose tracking is reliable.
3. Run BFS on **known traversable cells** toward observed or remembered goals.
4. Perform flag interaction or opponent-aware responses only under confirmed host rules.
5. Fall back to obstacle-aware local movement if memory or route confidence is insufficient.

A* and D* Lite remain optional later comparison candidates, not prerequisites. These are hypotheses/design proposals, not implemented competition capabilities.

## Core supporting documents

- [Literature summary](literature-summary.md) — existing academic work and its limitations.
- [Algorithm comparison](algorithm-comparison.md) — methods, trade-offs and current agent status.
- [Research plan](research-plan.md) — research questions and experimental phases.
- [Evaluation metrics](evaluation-metrics.md) — definitions and reproducibility controls.
- [Week 4 competition strategy](week4-competition-strategy.md) — rationale, assumptions, next steps.

- [FSM Design](fsm-design.md) — guarded game-state control (proposed).
- [Software Development Roadmap](software-development-roadmap.md) and [Research Roadmap](research-roadmap.md) — separate implementation and evaluation paths.
