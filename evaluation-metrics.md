# Evaluation Metrics — Unknown-Map CTF Competition

**Owner:** Nirmal Mohan (research and evaluation); technical review: Aiswarya Payakkal Vinayan.  
**Status:** Proposed measurement protocol; no match or navigation results are claimed.

## Principle

Keep separate (a) *requested actions*, (b) *verified movements/game events* and (c) *inferences*. The supplied API does not itself provide absolute player position or proof that `lastAction` succeeded. Record only observable outcomes.

| Metric | Operational definition | When feasible |
|---|---|---|
| DLL success | Host loads and completes test runs without crash | Now |
| Valid action rate | Defined `ACTION_*` codes / total calls to `Agent_Step` | Now |
| Known-blocked request rate | Moves aimed at visibly known wall or occupied neighbor / movement requests | With snapshot logger |
| Immediate requested reversal rate | Immediately opposite direction requested after earlier movement request / eligible consecutive transitions | With action logger; **not actual revisits** |
| Decision latency | Mean, median and 95th-percentile `Agent_Step` duration | With timer/instrumentation |
| Known-grid route length | Traversable steps in a confirmed BFS route to a known destination | Synthetic BFS fixtures |
| Time to first objective | Number of turns before first reliably detected flag/target | Once observations and events are logged |
| Coverage | Unique verified map positions / known reachable positions | After reliable localization |
| Actual revisit rate | Verified visits to already visited map cells / successful moves | After reliable localization |
| Flag delivery and win rate | Verified deliveries or won completed matches / comparable completed matches | Only if host exposes an unambiguous result |
| Robustness/generalization | Distribution of outcomes across distinct held-out maps/opponents, including failures | After repeated comparable evaluation |

| FSM transition validity | Valid, evidence-backed transitions / attempted transitions | Synthetic transition tests; host verification later |
| Recovery correctness | Uncertain-map or invalid-route scenarios that return a defined safe action / applicable scenarios | Synthetic fixtures before match tests |
| Frontier target reachability | Selected frontiers reachable through confirmed known traversable cells / selected frontiers | After reliable map/pose tracking |

## Comparability checklist

1. Record source revision, DLL build, host/API version, map ID/layout class and opponent policy.
2. Log random seeds, starting conditions, number of turns and repetitions when these can be controlled.
3. Use separate development and held-out unknown maps. **Do not obtain or tune using the actual competition map.**
4. Use equal movement-direction controls where possible; the original random agent uses eight compass moves while the safe policy initially uses four.
5. Include failed runs; do not compare samples from unlike game conditions as if randomized trials.
6. Report raw counts alongside rates; avoid claims of statistical superiority without sufficient replication.

**Pilot plan (not completed):** aim for ten comparable trials per working agent/map/opponent combination, revising this if the host does not support repeatability. Record why any metric was unavailable.

## Suggested CSV fields

`run_id,run_date,agent_commit,agent_variant,host_version,map_id,held_out,opponent_type,seed,turns,step_calls,valid_actions,known_blocked_requests,reverse_requests,fsm_state,verified_transition,recovery_reason,frontier_selected,bfs_route_valid,crashed,goal_first_seen_turn,verified_flag_events,verified_winner,notes`

## Reporting

Summarize outcomes by map and opponent, then across maps. State the sample size, mean/median and range or confidence interval as appropriate, event observability limits, unsuccessful cases and design changes. Do not fabricate measurements.

## Planned ablation comparisons (not results)

1. Original random control and safe movement baseline; when comparing filtering, match permitted directions.
2. Safe local novelty fallback versus map-based FBE, only if map localization is credible.
3. FBE without known-route BFS versus FBE plus BFS on comparable known-route opportunities.
4. FBE+BFS without explicit FSM versus FBE+BFS+FSM on flag/opponent scenarios, holding the underlying movement policy fixed where possible.
5. Optional A* versus BFS route search on equivalent known graphs **only after the core agent works**.

Report actual map/agent/opponent sample sizes, flag-state observability and failures, not merely winning screenshots.
