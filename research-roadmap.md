# Research Roadmap — Competition on Previously Unseen Maps

**Owner:** Nirmal Mohan (research/testing/evaluation); Aiswarya Payakkal Vinayan is technical reviewer.  
**Period:** October 2026–January 2027 (provisional).  
**Status:** protocol and hypotheses; **no measured win-rate improvements are yet established**.

## Research aim and main question

**Main question:** *Does the proposed FBE + BFS + FSM hybrid, with safe local fallback, improve objective discovery, robustness and competition performance on previously unseen maps compared with simpler movement and exploration strategies?*

The agent receives a local 5×5 snapshot without global coordinates or a guaranteed successful previous move. The opposing agent and map layout must be treated as unknown until observable. The team must **never use the actual secret competition map for tuning**.

## Hypotheses and tests (these are predictions, not results)

| ID | Hypothesis | Comparison | Primary evidence |
|---|---|---|---|
| H1 | Safe obstacle filtering reduces requests into *visible known* walls relative to a direction-matched random baseline | Random 4-way vs safe 4-way; original 8-way separately | Known-blocked request rate and valid action rate |
| H2 | Verified FBE explores more effectively than local novelty movement when mapping works | Local novelty fallback vs FBE on independently generated unknown maps | Discovery time and verified coverage when pose is reliable |
| H3 | BFS improves route efficiency toward a **known reachable** objective | FBE with direct movement choice vs FBE+BFS, with equal movement rules | Route length, goal-reaching success and decision latency |
| H4 | A guarded FSM improves objective completion/recovery over the same navigation without explicit game states | FBE+BFS without FSM vs FBE+BFS+FSM | Verified flag actions, transition errors, recovery success |
| H5 | A full hybrid generalizes better than baselines to unfamiliar maps/opponents | Safe baseline, local exploration, and tested hybrid variants | Completed matches and win rate **only when observable**; failures by map/opponent |
| H6 (optional) | A* saves search work on larger trusted maps | BFS vs A* under same graph, costs and goal | Nodes expanded / compute latency; only after core goals met |

## Research path and deliveries

| Research stage / period | Work package | What to do | Research artifact / gate |
|---|---|---|---|
| **R0 — Initial evidence** · Oct | WP-06 | Review Yamauchi frontier exploration, Korf online decisions, Sturtevant grid benchmarks; document reading depth | Literature summary and explicit *source claim vs project assumption* table |
| **R1 — Question and protocol design** · Oct–Nov | WP-06/07 | Finalize hypotheses, map categories, opponent profiles, event observability, sample size and raw logs | [Evaluation Metrics](evaluation-metrics.md), independent test plan and blank CSV |
| **R2 — Verify what the host reveals** · early Nov | WP-07/08 with WP-02 | Confirm illegal movements, flag possession, winner reporting and repeatable map/opponent selection | Rules/observation test log; unavailable metrics marked deferred |
| **R3 — Baseline measurements** · Nov–Dec | WP-08 | Keep original 8-way random and tested safe agent; optionally create four-way random control; repeat matched scenarios | Raw data and baseline tables (including failed runs) |
| **R4 — Synthetic component tests** · Nov–Dec | WP-08/09 | Validate frontier reachability, BFS routes, FSM guards, uncertain map recovery | Unit-test fixtures; distinguish route path cost from full-game results |
| **R5 — Controlled ablation tests** · Dec–Jan | WP-09 | Compare local fallback, FBE, FBE+BFS and FBE+BFS+FSM when implemented and validated | Matched-map/opponent runs, per-component effect and limitations |
| **R6 — Held-out competition-style evaluation** · Jan | WP-09/10 | Test against varied opponents on distinct unseen maps; avoid leakage from development maps | Per-map outcome distribution, success/failure counts and robustness analysis |
| **R7 — Interpretation and reporting** · Jan | WP-10/13 | Answer each RQ, assess alternative explanations and report limitations and reproducibility | Research report, methods, figures, exact build revisions and conclusions |

## Test-map and opponent policy

Use *development maps* for debugging (open, narrow corridors, loops, dead ends, blocked passages, multiple routes) and reserve separate **held-out maps** created independently or generated with undisclosed seeds for final evaluation. Record map size, terrain properties, version/seed when allowed, opponent identity or policy, turn limit, start conditions and host revision.

A win-rate comparison is only meaningful if wins and completed matches are observable. If not, report measurable surrogate outcomes without calling them wins. **A 5×5 observation is not a global map**, and approximate pose does not justify claiming exact exploration coverage.

## Fairness and analysis

- Compare runs on matched maps, rules, action sets, opponent policy and turn limits when practical; document anything that cannot be controlled.
- Different random/safe baselines choose eight versus four directions. Use a four-way random control for a causal obstacle-filter test.
- Plan around **10 repeated runs per agent/map/opponent** as an initial feasibility target, not as a guarantee of statistical significance. Record actual counts and failures.
- Retain raw logs and the exact source commit. Separate **requested** movements, **verified** game events and uncertain **inferences**.
- Summarize distributions across maps rather than reporting only best-case matches. Include failure cases, decisions that could not be measured and threats to validity.

## Literature foundation and limits

1. **Yamauchi (1997), _A Frontier-Based Approach for Autonomous Exploration_**: supports targeting known/unknown map boundaries, but its robot-mapping conditions do not directly validate our agent's pose estimate. https://doi.org/10.1109/CIRA.1997.613851
2. **Korf (1990), _Real-Time Heuristic Search_**: supports studying repeated observe/plan/act decisions. https://doi.org/10.1016/0004-3702(90)90054-4
3. **Sturtevant (2012), _Benchmarks for Grid-Based Pathfinding_**: motivates standardized comparable grid tests. https://doi.org/10.1109/TCIAIG.2012.2197681
4. **Hart, Nilsson & Raphael (1968)** provides A* theory for **optional** later comparison. https://doi.org/10.1109/TSSC.1968.300136

**FSM choice:** A game-state controller is an engineering design proposal, not a proven finding from the four sources above. Seek an additional relevant source and critically review its actual content before attributing research claims to it. The existing annotated literature survey documents six papers, with deep full-text review still to be completed.

## What Nirmal needs to deliver next (Week 4)

1. Dated literature findings, with accurately identified reading scope.
2. Explicit **FBE + BFS + FSM + safe fallback** design choice and unverified assumptions.
3. Metrics and map/opponent test strategy distinguishing current measurable outcomes from deferred ones.
4. A five-minute team progress presentation, research segment, and open questions for Professor Hipp.
5. GitHub Issue/diary references and Aiswarya's technical review.

**Related:** [Software Development Roadmap](software-development-roadmap.md) · [FSM Design](fsm-design.md) · [Algorithm Comparison](algorithm-comparison.md) · [Research Plan](research-plan.md).
