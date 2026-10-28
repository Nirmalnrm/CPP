# Literature Summary — Navigation and Exploration Research

**Project:** C++ Agent Project — WHZ Capture The Flag  
**Module:** Autonomous Research Project (KFT11050), coordinated with Software Project (ELT10210)  
**Research lead:** Nirmal Mohan  
**Software/project-planning reviewer:** Aiswarya Payakkal Vinayan  
**Review status:** Initial annotated literature survey. The publication details and available publisher/author summaries were checked; an in-depth, page-by-page review of each paper remains a research task.

## 1. Scope and selection method

The research problem is autonomous movement in a grid environment when the agent sees only a 5 × 5 local snapshot. Our initial safe agent could reject nearby obstacles but appeared to revisit a limited region. We therefore selected literature on **unknown-environment exploration, search/pathfinding, online decisions, replanning, and experimental benchmarking**.

Selection criteria: (1) an identifiable archival academic publication, (2) a clearly described method or evaluation contribution, (3) relevance to at least one of our research questions, and (4) a verifiable publisher/author/DOI link. This document does not imply that any of these algorithms has been implemented in our DLL.

## 2. Paper summaries

### [R1] Yamauchi (1997) — A Frontier-Based Approach for Autonomous Exploration

**Reference:** Yamauchi, B. (1997). *A Frontier-Based Approach for Autonomous Exploration*. Proceedings of the IEEE International Symposium on Computational Intelligence in Robotics and Automation (CIRA '97), pp. 146–151.  
**DOI:** https://doi.org/10.1109/CIRA.1997.613851  
**Author paper:** https://www.robotfrontier.com/papers/cira97.pdf

- **Research problem:** How can an autonomous system explore an initially unknown environment without repeatedly remaining in familiar territory?
- **Key idea:** Treat the boundary between already known traversable space and unexplored space as a **frontier**, and choose frontier regions as exploration objectives. The original work describes mapping and frontier navigation in robotic environments.
- **Relevance to our project:** Our `SafeMovementAgent` checks only adjacent cells. A frontier-based policy could provide a reason to travel into unexplored regions rather than making locally safe but repetitive choices.
- **Important limitation:** The method presumes a maintained map with spatial consistency. The supplied `WorldSnapshot` gives a local view and `Agent_Create` gives dimensions, but no absolute position. A dependable position/motion estimate is therefore a prerequisite for a persistent frontier map.
- **Potential research activity:** Investigate a local 'unexplored-neighbor' heuristic first; consider global frontiers only after movement-success detection and localization are validated.
- **Related questions:** RQ2, RQ4.

### [R2] Hart, Nilsson & Raphael (1968) — A Formal Basis for the Heuristic Determination of Minimum Cost Paths

**Reference:** Hart, P. E., Nilsson, N. J., & Raphael, B. (1968). *A Formal Basis for the Heuristic Determination of Minimum Cost Paths*. IEEE Transactions on Systems Science and Cybernetics, 4(2), 100–107.  
**DOI:** https://doi.org/10.1109/TSSC.1968.300136  
**Authors' manuscript:** https://ai.stanford.edu/~nilsson/OnlinePubs-Nils/PublishedPapers/astar.pdf

- **Research problem:** How can pathfinding use domain information to guide the search toward low-cost routes?
- **Key idea:** Incorporate heuristic estimates into graph search, providing the theoretical basis for what is commonly called **A\***.
- **Relevance to our project:** If the agent knows a destination and has a consistent graph of traversable cells, A* could select routes toward a flag or target. It can be compared with BFS on the same graph.
- **Important limitation:** A* does not reveal unknown parts of the world by itself. It also relies on clearly specified movement costs and an appropriate heuristic. For four-direction unit-cost movement, Manhattan distance is an appropriate lower bound; changing to diagonals requires reassessing costs and heuristics.
- **Potential research activity:** Implement a small, synthetic-grid comparison of BFS and A* after the movement graph and allowed directions are confirmed.
- **Related questions:** RQ3, RQ5.

### [R3] Korf (1990) — Real-Time Heuristic Search

**Reference:** Korf, R. E. (1990). *Real-time heuristic search*. Artificial Intelligence, 42(2–3), 189–211.  
**DOI:** https://doi.org/10.1016/0004-3702(90)90054-4

- **Research problem:** How can an agent make decisions while planning and acting are interleaved, rather than completing a full long-range search before every action?
- **Key idea:** Real-time heuristic search uses bounded deliberation and interleaves search with execution; the paper includes Real-Time-A*.
- **Relevance to our project:** The host requests one action per `Agent_Step()` call. This makes incremental decisions and replanning after new observations a natural design choice.
- **Important limitation:** The paper's assumptions and guarantees do not automatically transfer to our partially observed game. A movement may fail, a target may be unknown, and the local snapshot alone is not a complete graph.
- **Potential research activity:** Compare a low-computation local policy with a local planning policy; track per-step decision latency and behavior on changing snapshots.
- **Related questions:** RQ3, RQ5.

### [R4] Koenig & Likhachev (2002) — D* Lite

**Reference:** Koenig, S., & Likhachev, M. (2002). *D* Lite*. Proceedings of the Eighteenth National Conference on Artificial Intelligence (AAAI-02), pp. 476–483.  
**Institutional publication record:** https://publications.ri.cmu.edu/d-lite  
**Conference record:** https://ocs.aaai.org/Library/AAAI/2002/aaai02-072.php

- **Research problem:** How can a navigation system efficiently revise its route when information about terrain changes or is revealed?
- **Key idea:** Incremental heuristic search reuses previous computations to replan after changes in the known graph.
- **Relevance to our project:** The game exposes terrain incrementally as the agent moves; a replanning method could eventually make use of a remembered map.
- **Important limitation:** D* Lite is overpowered for a first 5 × 5 local experiment and needs a meaningful persistent map and localization. It is a **later research candidate**, not a Week 3 deliverable.
- **Potential research activity:** Keep incremental replanning as an optional later research comparison only after state tracking, the FBE/BFS/FSM core and its tests are reliable; there is no mandatory incremental-planning work package.
- **Related questions:** RQ4, RQ5.

### [R5] Sturtevant (2012) — Benchmarks for Grid-Based Pathfinding

**Reference:** Sturtevant, N. R. (2012). *Benchmarks for Grid-Based Pathfinding*. IEEE Transactions on Computational Intelligence and AI in Games, 4(2), 144–148.  
**DOI:** https://doi.org/10.1109/TCIAIG.2012.2197681  
**Benchmark repository:** https://www.movingai.com/benchmarks/grids.html

- **Research problem:** How can researchers compare grid-search methods when each study uses different maps and test problems?
- **Key idea:** Supply shared grid benchmarks and encourage systematic characterization and comparisons across scenarios.
- **Relevance to our project:** A single screenshot is not evidence that one algorithm is superior. Our experiments should use comparable layouts, map rules, run lengths and logging, and should report unsuccessful runs instead of hiding them.
- **Important limitation:** Benchmark maps may not exactly represent the professor's game mechanics; the game's moving adversary, pickup/drop-off rules, and local visibility introduce additional constraints.
- **Potential research activity:** Establish a set of small fixed snapshot fixtures for unit tests; run practical agent comparisons under matched game conditions whenever possible.
- **Related questions:** RQ1, RQ2, RQ5.

### [R6] Koenig, Likhachev & Furcy (2004) — Lifelong Planning A*

**Reference:** Koenig, S., Likhachev, M., & Furcy, D. (2004). *Lifelong Planning A**. Artificial Intelligence, 155(1–2), 93–146.  
**DOI:** https://doi.org/10.1016/j.artint.2003.12.001  
**Institutional record:** https://publications.ri.cmu.edu/lifelong-planning-a

- **Research problem:** How can repeated related shortest-path problems be solved without repeating all computation after small graph changes?
- **Key idea:** Lifelong Planning A* reuses parts of a previous search when the graph is updated.
- **Relevance to our project:** This provides research background for efficient navigation if the agent eventually maintains a stable global graph that gains new information over time.
- **Important limitation:** The current agent lacks reliable absolute position and a persistent explored graph. Implementing LPA* now would add complexity without resolving those prerequisites.
- **Potential research activity:** Retain as a longer-term comparison to D* Lite and conventional A*.
- **Related questions:** RQ4, RQ5.

## 3. Comparative synthesis

| Paper | Best contribution to our research | Read/implementation priority |
|---|---|---|
| R1 — Yamauchi | Avoiding repeated exploration by seeking known/unknown boundaries | **High — concept for RQ2/RQ4** |
| R5 — Sturtevant | Fair scenario design and reproducible evaluation | **High — experiment design** |
| R3 — Korf | Online action selection with incremental planning | High — decision architecture |
| R2 — Hart et al. | Heuristic pathfinding theory for a later A* implementation | Medium |
| R4 — D* Lite | Replanning when map information changes | Later |
| R6 — LPA* | Reuse of previous search work | Later |

**Week 3 initial conclusion (not an experimental result):** The first plan prioritized movement verification, reproducible baselines and local BFS for visible goals. **Week 4 refines this plan** because the competition map is initially unknown: safe online exploration and a tested movement-state fallback come first, FBE selects exploration targets when mapping is credible, BFS routes through known traversable cells, and an FSM coordinates objective behavior.


## 4. Week 4 competition-specific synthesis (proposed)

**New practical constraint:** The contest map is unknown before a match and a different team's agent participates. This is a user-reported competition condition, **not a result established by the six research papers**. The following adaptation of the literature is therefore a project hypothesis.

| Existing source | Literature-based concept | Proposed application | Limitation to verify |
|---|---|---|---|
| R1 Yamauchi (1997) | Exploration targets known/unknown frontiers | Seek informative unexplored boundaries on a newly discovered map | Requires reliable spatial map and localization |
| R3 Korf (1990) | Real-time/online decisions | Reconsider movement each `Agent_Step` under new observations | Host rules and action outcomes must be validated |
| R5 Sturtevant (2012) | Standardized grid benchmark scenarios | Compare policies on varied held-out maps with consistent conditions | Benchmarks alone do not reproduce a game with opponents and flags |
| R2 Hart et al. (1968) | Heuristic shortest-path search | Compare A* with BFS when a reliable known graph exists | Neither identifies a hidden objective automatically |
| R4/R6 incremental search | Reusing search effort after map updates | Optional future replanning | Too early without stable pose and map state |

**Preliminary decision:** prioritize safe **online exploration and robust fallback**; use BFS for goal-directed routes within known traversable cells; investigate flag/opponent behavior with the provided API. A* is not a required primary strategy. No source in this survey establishes a guaranteed win-rate advantage in this particular Capture The Flag game.

**Actual reading log to complete by Nirmal:** For each paper studied further, add date, full-text sections/pages consulted, the precise claim supported and the remaining question. Do not state that full articles were critically reviewed until that work has been completed.

## 5. FSM orchestration and literature boundaries (28 October proposal)

**Finite-State Machine is a software architecture choice**, rather than a navigation algorithm whose superiority follows from R1–R6. It makes behaviors and transitions explicit, supports unit testing, and separates exploration decisions from route planning. The current six papers do not experimentally validate this particular FSM controller against competition opponents. This must be evaluated through our own controlled comparisons.

**Next reading task:** locate and critically review a suitable academic or authoritative source on finite-state controllers for game agents or reactive robots; record pages/sections actually read before adding it as a formal reference. Retain Yamauchi (frontier concept), Korf (online decisions) and Sturtevant (benchmark methodology) as initial verified research directions.

[Research Roadmap](research-roadmap.md) defines the reading, hypothesis and experiment sequence.
