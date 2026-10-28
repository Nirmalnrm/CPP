# Project Description — C++ Capture The Flag Agent

**Modules:** Software Project (ELT10210) and Autonomous Research Project (KFT11050)  
**Team:** Aiswarya Payakkal Vinayan (Software Engineering) and Nirmal Mohan (Research and Evaluation)  
**Status:** Week 4 strategy revision, proposed for team and supervisor review; no hybrid-agent results yet.

## Background and objective

The team is developing an autonomous **C++ Windows DLL agent** for the professor-provided WHZ Capture The Flag graphical host. The DLL receives a **5 × 5 local `WorldSnapshot`** through the supplied API and returns one permitted action per call. The graphical host and the Week 2 API remain unchanged.

The competition will involve an **unknown map** and an opposing team's agent. Because the agent cannot rely on knowing the map before the match, it must explore available space, recognize observed objectives, navigate using only justified information and react to visible opponents under confirmed game rules.

The professor's `StupidAgent` is a random-movement example; our `SafeMovementAgent` checks nearby known traversable cardinal cells. These two agents have been loaded and tested in the graphical host, but the safe agent has shown repetitive movement during initial observation. Quantitative competition results have **not** yet been collected.

## Preliminary competition strategy — Week 4

The team proposes a **hybrid architecture** consisting of:

1. **Frontier-Based Exploration (FBE):** identify reachable boundaries between known traversable cells and unknown space **only if spatial mapping and position estimation have been validated**.
2. **Breadth-First Search (BFS):** calculate routes to *known, reachable* exploration frontiers, visible flags or remembered objectives on a known, unweighted traversable graph.
3. **Finite-State Machine (FSM):** manage objectives such as explore, approach a flag, pick up, return, deliver, respond to a visible opponent and recover. Transitions requiring flag possession or movement success must be supported by confirmed host behavior.
4. **Safe local exploration fallback:** use obstacle-aware local decisions or `ACTION_WAIT` when location, map or route confidence is insufficient.

**A*** is an **optional later comparison**, not a required core algorithm. FBE chooses where to explore, BFS chooses how to navigate known routes, and the FSM decides which game objective takes priority.

This architecture is **proposed**, not implemented or experimentally proven. The first development priority is validating movement and interaction rules; no source code change to the existing reference agents is required for the Week 4 review.

## Software Engineering and Research

The **Software Engineering** module will design and test DLL-compatible components, movement/outcome tracking, safe fallback, map/frontier selection, BFS routing and an FSM controller. The **Research** module will review relevant academic literature, define falsifiable hypotheses, design controlled experiments on multiple previously unseen maps, compare available agent variants and analyze robustness and game outcomes when these can be observed reliably.

The planned period is **19 October 2026–31 January 2027**, with **14 work packages and 300 estimated person-hours**. Dates after the October planning review are provisional and must be confirmed against course deadlines. The objective is a documented, reliable agent supported by reproducible research, not a guaranteed competition victory.

See [Project Plan](project-plan.md), [Project Schedule](project-schedule.md), [Research Plan](research-plan.md), [Week 4 Strategy](week4-competition-strategy.md), [Software Roadmap](software-development-roadmap.md) and [Research Roadmap](research-roadmap.md).
