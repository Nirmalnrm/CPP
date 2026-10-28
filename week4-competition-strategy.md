# Week 4 — Competition-Oriented Research Decision (Proposed)

**Date prepared:** 28 October 2026  
**Next professor meeting:** 30 October 2026  
**Proposed by:** Research Lead (Nirmal), for review with Software Lead (Aiswarya).

## New information

The final game is a competition on a **previously unseen map** built by another person, involving at least one opposing team's agent. This requirement was communicated by the team. **The professor's complete competition rules and opponent/score API are not yet confirmed.**

## Design consequence

BFS and A* solve route-finding **once a traversable graph and destination are known**. They do not by themselves decide where to explore before finding a flag. The agent therefore needs an **online exploration and objectives policy**, not only a path planner.

## Preliminary choice

Adopt a **proposed hybrid: Frontier-Based Exploration (FBE) + BFS + Finite-State Machine (FSM), with safe local exploration fallback**. A* remains optional.

- **FBE (where to investigate):** choose reachable known-floor frontier cells bordering unknown territory, only when localization and map memory are sufficiently reliable. Otherwise use local novelty/obstacle-aware fallback.
- **BFS (how to get there):** compute a valid path through known traversable cells toward a frontier or observed/reliably remembered flag/target; return one action per `Agent_Step`.
- **FSM (what to do next):** switch between `EXPLORE`, `APPROACH_FLAG`, `PICKUP`, `RETURN`, `DELIVER`, `EVADE` and `RECOVER` using verified observations/events; **no assuming flag pickup or movement success**.
- **Fallback:** on bad pose/map confidence, an unreachable target or uncertain opponent rule, clear the unsafe plan and use safe local movement or `ACTION_WAIT`.

**Status:** preliminary architecture and research hypothesis only; original DLL baselines remain the only demonstrated agent implementations. See [FSM Design](fsm-design.md).

## Literature evidence versus assumptions

| Supported concept | Competition hypothesis or unverified assumption |
|---|---|
| Frontier selection can target known/unknown boundaries on a consistent map | Such selection will outperform safe movement on this game's hidden maps |
| BFS gives shortest paths on known unit-cost graphs | Discovered goals can be reached reliably using available movement rules |
| Real-time methods interleave decisions and new observations | Online adaptation will outperform a static policy against a particular opponent |
| Reproducible grid benchmarks help compare methods | Held-out test maps will predict tournament performance |

## Implementation and experimental next steps

- **Aiswarya:** verify API and movement outcomes; design a confidence-aware state tracker; test a safe exploration fallback; implement known-grid BFS as an internal planner; clarify flag/opponent rules.
- **Nirmal:** deepen literature review; define measurable hypotheses; build varied development + untouched held-out maps; devise baseline and opponent-controlled test procedures; log results and failures.
- **Both:** update project plan and 300-hour schedule; review GitHub Issues; seek instructor guidance on winner feedback and movement semantics.

## Week 4 deliverables

Initial literature findings; a reasoned **provisional strategy**; requirement/schedule updates; a five-minute progress presentation; a dated developer diary record. **A finished competition algorithm is not required for October 30.**

## Questions to confirm with professor

1. How can the agent distinguish successful and blocked moves?
2. Are diagonal corner moves legal and what do they cost?
3. How is flag possession, pickup, delivery or scoring reported?
4. What happens when agents meet? What actions can opponents take?
5. Can experiments repeat identical maps and seeds? Is the match winner observable?
6. Are there fixed memory, time, resource or map-size limits?

## Separate execution paths

- [Software Development Roadmap](software-development-roadmap.md) — owner Aiswarya: implementation gates and testable modules.
- [Research Roadmap](research-roadmap.md) — owner Nirmal: literature, baselines, comparison design and publication-quality evidence.
