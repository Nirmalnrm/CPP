# Proposed Finite-State Machine — Competition Agent

**Status:** Design, **not implemented**. The Week 2 DLL's public five-function API remains unchanged.  
**Owner:** Aiswarya (software); review/test design: Nirmal (research).

## Why an FSM?

Frontier exploration chooses where to gather information. BFS calculates a route **once a known reachable target exists**. Neither defines the entire competitive objective lifecycle. The FSM coordinates game goals and recovery, providing explicit transitions that can be unit-tested independently.

## Proposed states and actions

| State | Purpose | Action policy | Transition guard / evidence |
|---|---|---|---|
| `EXPLORE` | Discover useful unknown regions | Trusted frontier via BFS, otherwise safe local exploration | Verified visible enemy flag or known reachable objective → `APPROACH_FLAG` |
| `APPROACH_FLAG` | Reach a verified known flag location | BFS through known legal cells; fallback when route disappears | Confirmed agent at legal pickup position → `PICKUP`; unreachable/unknown → `EXPLORE` or `RECOVER` |
| `PICKUP` | Attempt pickup under host rules | `ACTION_PICK_UP_FLAG` only if allowed | **Verified possession/status** → `RETURN`; without confirmation, reobserve/retry conservatively |
| `RETURN` | Reach verified delivery destination | BFS toward known own target; explore locally if destination unknown | Confirmed agent at legal delivery cell → `DELIVER` |
| `DELIVER` | Attempt delivery under host rules | `ACTION_PUT_DOWN_FLAG` only when legal | **Verified delivery or state change** → `EXPLORE` or result-handling state |
| `EVADE` | Respond to visible dangerous/blocked opponent situations | Legal safe local maneuver and optional route replan | Visible threat resolved and prior plan still valid → prior relevant state |
| `RECOVER` | Repair or abandon invalid state/map assumptions | Invalidate unsafe route; safe local exploration or wait | Consistent observations and trusted plan restored → appropriate objective state |

**Pending rule questions:** Does the host reveal possession? Is pickup automatic or explicitly requested? Which target completes delivery? Can enemy occupancy harm the agent? There is no unconditional transition from `PICKUP` to `RETURN` merely because the action was requested.

## Transition priorities

1. Reject unsupported action codes and check for failed/contradictory pose assumptions; enter `RECOVER` if needed.
2. Use only confirmed interactions/events to choose `PICKUP`, `RETURN` and `DELIVER`.
3. Observe enemy occupancy; allow `EVADE` only with host-defined enemy effects.
4. If a flag/objective is visible or reliably mapped, prioritize a known-route objective.
5. Otherwise `EXPLORE`, choosing a reachable frontier only with valid map state.
6. If there is no reliable route, fall back to safe local movement or `ACTION_WAIT`.

## One-turn pseudocode

```text
step(snapshot, lastAction):
    obs = decode(snapshot)
    update_pose_only_if_verifiable(obs, lastAction)
    if pose_confidence_invalid or route_conflicts_with_observation:
        state = RECOVER
    else:
        state = guarded_transition(state, obs, confirmed_events)

    if state == RECOVER:
        action = local_safe_explore_or_wait(obs)
    else if state == EXPLORE:
        if trusted_map and reachable_frontier:
            action = BFS_first_step_to_frontier()
        else:
            action = local_safe_explore_or_wait(obs)
    else if state in {APPROACH_FLAG, RETURN}:
        action = BFS_first_step_to_verified_known_goal()
        if no route: action = local_safe_explore_or_wait(obs)
    else if state in {PICKUP, DELIVER}:
        action = interaction_if_confirmed_legal_else_wait()
    else if state == EVADE:
        action = safe_visible_opponent_response_or_wait()

    return verify_defined_API_action(action)
```

## Unit tests required before host integration

- A known wall, occupied cell or unknown cell is never treated as confirmed traversable terrain.
- `EXPLORE` with no trusted global map takes a valid safe local action, or waits.
- Known frontier is traversable and adjacent to an unknown cell; unreachable frontier is rejected.
- BFS returns a shortest route on a known unit-cost grid and fails safely on blocked target.
- A requested but unconfirmed pickup **does not** set possession or force `RETURN`.
- A contradictory snapshot invalidates the route and enters `RECOVER` without invalid commands.
- Reset restores `EXPLORE` and clears game-specific targets/state; no stale opponent/flag assumptions persist.
- All transitions/outputs are logged with state, guard reason, source revision and fixture ID.

## Optional A* extension

A* may replace the BFS route planner only after movement costs, legal directions and heuristic admissibility are established, and after the FBE+BFS+FSM agent runs reliably. It is not required for Week 4 or the baseline competition strategy.

**Related:** [Software Development Roadmap](software-development-roadmap.md) · [Research Roadmap](research-roadmap.md) · [Project Requirements](project-requirements.md).
