# Software Development Roadmap — Unknown-Map CTF Agent

**Owner:** Aiswarya Payakkal Vinayan (software lead); Nirmal Mohan reviews experimental testability.  
**Planning horizon:** 19 October 2026–31 January 2027 (provisional).  
**Proposed core:** **Frontier-Based Exploration (FBE) + BFS + Finite-State Machine (FSM)** with **safe local exploration fallback**. **A*** is optional.  
**Status as of 28 October:** design and roadmap only; current working agent provides basic safe local movement.

## Architecture roles — do not confuse the algorithms

| Layer | Question answered | Strategy | Hard condition |
|---|---|---|---|
| Sensor and safety | What is visible and currently traversable? | 5×5 snapshot decoder; obstacle filter | Unknown cells are not confirmed floor |
| Movement state | Do we know where we moved? | Observation comparison, uncertainty tracking | Do not treat `lastAction` as confirmed success |
| Exploration target | **Where should we discover more?** | FBE: reachable known-floor cell bordering unknown cells | Persistent FBE only when pose/map is consistent |
| Route planner | **How do we get to the chosen target?** | BFS on known legal, unit-cost cells | Target and route must be known/reachable; diagonal rules checked first |
| Behavior control | **What should we do now?** | FSM: explore, approach flag, pickup, return, deliver, evade, recover | Transition only on verifiable game facts or clearly marked inference |
| Recovery | What if assumptions fail? | Safe local novel move or `ACTION_WAIT` | Always available without global localization |
| Optional refinement | Can we plan faster on large trusted maps? | A* | Only after BFS/FBE/FSM stability and team approval |

## Step-by-step development path

| Stage / target | Work packages | Tasks to implement | Acceptance gate / deliverable |
|---|---|---|---|
| **S0 — Existing baseline** · through Week 3 | Existing | Preserve professor random agent and tested SafeMovementAgent under distinct filenames/commits | Both DLLs load in graphical host; baseline source remains unchanged |
| **S1 — API and rule verification** · late Oct–mid Nov | WP-01, WP-02 | Unit tests for cell masks, action codes, create/reset/step/destroy; test failed movement and collision/diagonal rules | Recorded API fixture tests and statement of what movement outcomes can/cannot be inferred |
| **S2 — Safe novelty fallback** · early/mid Nov | WP-02, WP-04 (early) | Reusable 4-way legal move filter; optional bounded last-direction/local novelty preference; `ACTION_WAIT` when no safe neighbor | Known wall/occupied/unknown never selected as confirmed traversable; works without global coordinates |
| **S3 — Map/pose confidence** · Nov–early Dec | WP-03 | Attempt observation-matching or validated relative position; store cell knowledge only when confidence is sufficient; invalidate contradictory maps | Corridor, blocked movement, repetitive/ambiguous view fixtures; recovery on contradictions |
| **S4 — Frontier target selection** · late Nov–Dec | WP-04 | Detect known reachable floor cells bordering unknown cells; filter unreachable targets; select informative frontier; gracefully fall back if no trusted map | Synthetic known/unknown grids; unreachable frontier never chosen; no fabricated global pose |
| **S5 — BFS on known routes** · Dec–Jan | WP-05 | BFS over documented legal movement edges, return first action; replan on new evidence; handle blocked/unknown goal | Shortest valid route on equal-cost fixtures; no path → fallback; same API untouched |
| **S6 — FSM game controller** · Dec–Jan | WP-05 | Implement states, transition guards, priorities, reset and flag/opponent safety rules in [FSM Design](fsm-design.md) | All synthetic state transitions tested; unconfirmed pickup/movement does not force RETURN |
| **S7 — Host integration and competition QA** · Dec–Jan | WP-12, WP-14 | Integrate exploration+BFS+FSM into a new agent source, leave baselines intact; test time, reset, opponent conditions and failure recovery | DLL loads repeatedly, never emits unknown action code, handles uncertainty and records test outcome |
| **S8 — Optional A* (conditional)** | Only if time remains | Compare pathfinding workload on larger *trusted* graphs, otherwise omit | No sacrifice of core features or experimental validity |

## Proposed directory structure (not current implementation evidence)

```text
tests/Week02_HostandAgent/
  agent_api.h              # professor-supplied, unchanged
  stupid_agent.cpp         # reference, unchanged
  safe_agent.cpp           # baseline, unchanged
src/                       # create as development proceeds
  competition_agent.cpp    # new future DLL entry and FSM orchestration
  snapshot_decoder.*
  local_fallback.*
  movement_tracker.*
  frontier_explorer.*
  bfs_planner.*
  fsm_controller.*
tests/                     # future isolated fixtures and automated validation
  snapshots/
  planning/
  fsm/
```

These `src/` files are a **proposed modular structure**; the current ZIP does not contain them. No build commands are given until the implementation and compatible compiler settings are established.

## FSM precedence and fallback

1. Check host-supplied observations and action legality; do not infer hidden enemy locations.
2. Clear invalid pose/map/route assumptions and enter `RECOVER` where required.
3. If a **confirmed** delivery/pickup action is legal, allow the corresponding state action.
4. If visible opposition changes a safe route, handle `EVADE` under confirmed collision rules.
5. If a trusted objective exists, BFS can plan to it; otherwise select a reachable frontier.
6. Without a usable target or reliable map, always use safe local exploration or `ACTION_WAIT`.

## Dependencies, collaboration and change control

- Nirmal supplies controlled fixtures, metric definitions and test expectations before comparative runs; Aiswarya provides versioned agent builds and test hooks without altering the professor's host.
- Aiswarya may implement a local exploration fallback *before* full-map FBE. Full-map FBE **must not** be treated as functioning without reliable pose validation.
- The shared Gantt remains **14 work packages, 300 planned person-hours**. This roadmap details scope, not extra hours.
- Maintain separate GitHub Issues/PRs for S1–S7 with implementation commits and test evidence; record only actual progress in the developer diary.

**Related:** [Project Plan](project-plan.md) · [Project Schedule](project-schedule.md) · [FSM Design](fsm-design.md) · [Research Roadmap](research-roadmap.md).
