# Research Plan — Autonomous C++ Capture The Flag Agent

**Module:** Autonomous Research Project (KFT11050), coordinated with Software Project (ELT10210)  
**Project:** WHZ Capture The Flag agent DLL  
**Researchers:** Nirmal Mohan (research lead); Aiswarya Payakkal Vinayan (software planning and technical reviewer)  
**Status:** Research proposal; experimental findings have **not** yet been collected.

## 1. Background and motivation

The professor-provided random `StupidAgent` and the team's obstacle-aware `SafeMovementAgent` provide working baseline policies, but they do not systematically explore or pursue flags. The supplied host gives only a **5 × 5 local** snapshot, and `lastAction` is not guaranteed to mean a successful movement.

**New Week 4 competition constraint:** A different person designs the competition map, which is not known to the agent before a match; our agent competes against an opposing team's agent. This information comes from the team and should be checked against final competition rules. It changes the main research problem from pathfinding on a known grid to **robust online exploration and objective-oriented play under partial observation and opponent interference**.

**Research aim:** evaluate whether an explicit **Frontier-Based Exploration + BFS + Finite-State Machine** design, protected by safe local fallback, improves unseen-map competition outcomes compared with reference movement policies. More specifically, study whether a conservative hybrid of safe local movement, novelty/frontier-inspired exploration (when memory is reliable), BFS routes to visible/remembered goals and observed-opponent responses generalizes to unseen maps. This is a proposal only; tournament results and winning performance are not yet known.

## 2. Objectives and research questions

**O1:** Establish random and safe-movement baselines using repeatable tests.  
**O2:** Evaluate exploration progress and goal discovery on unseen maps without leaking the secret competition layout.  
**O3:** Study movement-success uncertainty and the feasibility of persistent map memory.  
**O4:** Test whether known-route BFS and validated objective actions improve progression toward the flag.  
**O5:** Evaluate behavior across multiple opponent types and report uncertainty, failures and generalization.

| ID | Research question | Hypothesis (not yet tested) |
|---|---|---|
| RQ1 | Does novelty/frontier-inspired exploration discover more useful map area or objectives than safe random movement on unseen maps? | Information-seeking movement may improve discovery when position inference works. |
| RQ2 | How do blocked or uncertain moves affect map memory and navigation? | Confidence-aware tracking plus fallback should reduce catastrophic mapping errors. |
| RQ3 | When a reachable objective is known, does BFS improve route efficiency over safe random movement? | BFS should provide shortest known unit-cost paths under confirmed movement rules. |
| RQ4 | Can visible-opponent-aware decisions improve reliable objective progress? | Avoiding occupied/threatening routes may help, conditional on actual game rules. |
| RQ5 | Does the hybrid agent generalize across held-out maps and opponent policies? | A robust hybrid may outperform local policies, but results could depend on layout and opponents. |

See [Literature Summary](literature-summary.md), [Algorithm Comparison](algorithm-comparison.md), [Week 4 Competition Strategy](week4-competition-strategy.md), and [Evaluation Metrics](evaluation-metrics.md).

### New FSM-specific question and falsifiable hypothesis (Week 4)

**RQ6:** Does an observation-guarded finite-state controller improve objective completion and recovery over the same FBE+BFS navigation without game-level state selection? **H6 (untested):** The FSM may reduce goal-switching and unsupported actions, but can also fail if flag/possession events are inferred incorrectly. Compare against an otherwise identical variant without the FSM using observable events only.

**Implementation choices versus literature:** FBE is supported conceptually by Yamauchi; BFS follows standard shortest-path graph-search properties; Korf motivates online interleaving of decisions; the FSM is a **proposed engineering control structure**, not a measured result established by the six reviewed papers.

## 3. Literature search method

Search IEEE Xplore, ACM Digital Library, Google Scholar, and university/author publication pages using combinations of: `frontier exploration unknown maps`, `finite state machine autonomous game agents`, `online CTF strategy`, `grid pathfinding A*`, `autonomous frontier exploration partial map`, `real-time heuristic search`, `incremental replanning unknown terrain`, and `pathfinding benchmarks game maps`.

**Inclusion criteria:** peer-reviewed or archival conference/journal publications; a clear algorithm or evaluation methodology; relevance to grid maps, partial observation, exploration, replanning or experimental comparison. **Exclusion criteria:** unsourced tutorials as primary evidence, papers unrelated to discrete navigation, and works whose results cannot be adapted meaningfully to this API.

For each paper, record author, year, venue/DOI, key idea, potential adaptation, limitation, and whether the full text has been read. The publications below have been **identified and their bibliographic details checked**; identification is not the same as a completed in-depth literature review. Preliminary annotated contributions, project-specific limitations and full-text reading tasks are recorded in [literature-summary.md](literature-summary.md).

### Selected research papers

| Ref | Publication | Relevance to this project | Application priority |
|---|---|---|---|
| [R1] | B. Yamauchi (1997), *A Frontier-Based Approach for Autonomous Exploration*, IEEE CIRA, pp. 146–151. [DOI](https://doi.org/10.1109/CIRA.1997.613851) | Explores borders between known and unknown space; relevant to repeated-region behavior and eventual exploration memory. | **High** |
| [R2] | P. E. Hart, N. J. Nilsson, B. Raphael (1968), *A Formal Basis for the Heuristic Determination of Minimum Cost Paths*, IEEE Transactions on Systems Science and Cybernetics 4(2), pp. 100–107. [DOI](https://doi.org/10.1109/TSSC.1968.300136) | Foundational A* heuristic pathfinding; useful for later goal-directed navigation. | Medium |
| [R3] | R. E. Korf (1990), *Real-Time Heuristic Search*, Artificial Intelligence 42(2–3), pp. 189–211. [DOI](https://doi.org/10.1016/0004-3702(90)90054-4) | Interleaves decisions and execution; relevant when the host expects one action per call. | **High** |
| [R4] | S. Koenig, M. Likhachev (2002), **D\* Lite**, Proceedings of AAAI-02, pp. 476–483. [Paper](https://publications.ri.cmu.edu/d-lite) | Replanning on terrain that becomes known over time; possible later algorithm once map tracking exists. | Medium |
| [R5] | N. R. Sturtevant (2012), *Benchmarks for Grid-Based Pathfinding*, IEEE Transactions on Computational Intelligence and AI in Games 4(2), pp. 144–148. [DOI](https://doi.org/10.1109/TCIAIG.2012.2197681) | Motivates fair scenarios and consistent performance metrics for grid navigation experiments. | **High** |
| [R6] | S. Koenig, M. Likhachev, D. Furcy (2004), **Lifelong Planning A\***, Artificial Intelligence 155(1–2), pp. 93–146. [DOI](https://doi.org/10.1016/j.artint.2003.12.001) | Reuses search information when map costs change; advanced future work, not required for the initial prototype. | Low / future |

**Reading order:** R1 and R5 first (exploration and evaluation), followed by R3 (online decisions), R2 (A* theory), and R4/R6 if time permits.

Additional comparison of requirements, implementation statuses, validity threats and next-step tests appears in [algorithm-comparison.md](algorithm-comparison.md).

## 4. Candidate algorithms and implementation status

| Method | Role in competition | State as of revised proposal | Key limitation |
|---|---|---|---|
| `StupidAgent` | Eight-way random baseline | Demonstrated with graphical host | Does not avoid obstacles or find goals |
| `SafeMovementAgent` | Four-way obstacle-aware baseline | Demonstrated with graphical host; version details must be logged | No dependable map memory |
| Reverse-disfavoring safe policy | Low-cost local fallback | Source available in repository; no controlled improvement demonstrated | Can repeat long loops; prior action may fail |
| Local novelty heuristic | Select locally less-familiar moves | Proposed | Without absolute position, novelty may be only approximate |
| Frontier-based exploration | Select boundary of known/unknown map | Literature-backed proposal | Requires stable pose/map alignment |
| BFS on known traversable graph | Route planner to discovered goals | Proposed | Unknown goal cannot be routed to directly |
| Visible-opponent policy | Avoid known collisions or threats | Proposed; rules unverified | Host may not expose enough opponent information |
| A* / incremental replanning | Optional later comparison | Not a required first competition build | Requires consistent map/goal and added complexity |

**Provisional strategy decision:** explore while objectives are unknown; when goals and paths become known, plan with BFS; fall back to safe local behavior when state confidence is low. This is **not a claim of implementation or measured advantage**.

## 5. Research methodology and experiment design

### Phase A — Verify observable mechanics

Use controlled 5 × 5 snapshots and graphical-host runs to verify cell decoding, waiting, blockers, DLL lifecycle and action codes. Determine whether movement success can be detected, how diagonals behave, what flag pickup/delivery requires and what opponent state is visible. Treat unverified rules as assumptions.

### Phase B — Baseline and generalization test design

Test random and safe baselines using multiple authored development maps with corridors, loops, dead ends and open cells. Reserve **separate held-out maps** created without referencing the secret competition layout. Document opponent policy, allowed moves, agent revision, map ID, seed if controllable, start conditions, trial count and failures. Consider a four-direction random control to normalize direction-set differences.

### Phase C — Build and validate FBE + BFS + FSM incrementally

Implement/test a local novelty fallback first. If displacement inference is validated, attempt persistent map memory and frontier objective selection. Add BFS to route between known traversable cells when the goal position is known. Check any predicted movement against the next observation before trusting state updates. Unit-test synthetic maps before host integration.

**FSM verification (additional gate):** use synthetic event sequences for `EXPLORE`, `APPROACH_FLAG`, `PICKUP`, `RETURN`, `DELIVER`, `EVADE` and `RECOVER`; verify action validity and no transition based solely on an unconfirmed requested movement or flag action. Fallback must work without a global map.

### Phase D — Evaluate competition behavior

Run validated policies on **held-out maps** against multiple available opponent policies (e.g., reference/random, safe baseline, another controlled agent), recording score or win result only if verifiable. Compare controlled ablations: safe baseline, frontier policy without BFS, frontier+BFS with simple objective rules, and frontier+BFS+FSM, where all variants can be implemented reliably. Compare optional opponent-response subvariants separately to attribute improvements. Report dispersion, failed runs and rule uncertainties.

**Not permitted as research methodology:** hard-code or tune against the private final competition map.

More operational definitions appear in [Evaluation Metrics](evaluation-metrics.md).

## 6. Evaluation metrics and evidence

| Metric | Proposed operational definition | Availability |
|---|---|---|
| API/DLL stability | Runs with no load, lifecycle or step failure / attempted runs | Can test now |
| Valid action rate | Defined action code count / total `Agent_Step` calls | Can test now |
| Known-blocked request rate | Movement requests into **visible known** wall/occupied cells / movement requests | Can instrument now |
| Immediate requested reversal rate | Opposite movement requests / eligible action transitions | Can instrument now; not actual backtracking |
| Decision latency | Mean/median and upper percentile time per `Agent_Step` | Requires timer instrumentation |
| Objective discovery | Turns to first verified enemy-flag sighting | Requires repeatable observation logs |
| Coverage and physical revisits | Unique verified absolute cells / total reachable cells; revisit fraction | **Deferred** until localization is reliable |
| Flag capture/delivery and win rate | Verified successful game events or wins / completed comparable trials | **Deferred** until host event/result observable |
| Generalization robustness | Outcomes summarized across held-out maps and opponent policies, including failures | After comparable competition tests |
| FSM transition validity | Transitions justified by confirmed observation/event or explicit uncertainty guard / tested transitions | Synthetic FSM fixtures first; host telemetry later |
| Recovery/fallback activation | Number of uncertainty/invalid-route cases that return safely to local movement or wait | Synthetic contradictory-snapshot tests, then host logs |

**Pilot design, not completed:** target 10 comparable runs per agent/map/opponent combination when technically feasible; adjust based on available reproducibility. Record sample size rather than claiming a statistically certain win rate from a handful of matches. No numerical superiority is claimed.

See [Evaluation Metrics](evaluation-metrics.md) for detailed definitions and a logging template.

## 7. Software engineering and research integration

1. **API analysis** defines observable inputs and actions (software engineering).  
2. **Research questions** identify a behavioral weakness to investigate (research).  
3. **Helpers and policy variants** implement testable approaches (software engineering).  
4. **Repeatable tests and measurement** gather comparable evidence (both).  
5. **Results** guide the next algorithmic choice and changes to agent state or architecture (research feeding back into software engineering).

Keep the API exports stable. Separate any future snapshot decoder, movement validator, policy selection, and logging functionality as the code grows. Store experiment scripts, test fixtures, results, and links to source revisions in the repository. Record actual work and responsible contributor in `devlog/developer-diary.md`.

## 8. Risks, assumptions and open questions

- **Competition map unknown:** Do not use the actual secret layout as a development asset; evaluate on independent maps.
- **Localization uncertainty:** API does not supply absolute coordinates; a requested action does not confirm displacement.
- **Opponent rules uncertain:** Avoid assuming attacks, capture mechanics, flag possession or global opponent locations.
- **Direction-set confound:** Reference random agent uses eight moves; safe agent uses four. Normalize where possible.
- **Partially observed objectives:** BFS finds routes through known traversable cells; it does not independently discover an unseen flag.
- **Outcome measurement:** Win rate is measurable only when a verifiable result can be captured from the host.
- **Schedule risk:** Prioritize reliable movement and exploration before optional A* or incremental planners.

**Research status (Week 4 proposal):** References and design methods have been identified; no measured competition win-rate, exploration advantage or hybrid-agent implementation is claimed.

## 9. Research Path and Decision Gates

See the full [Research Roadmap](research-roadmap.md). The study is organized into: **literature and hypotheses → protocol and baselines → component unit tests → held-out map/opponent experiments → ablation tests → analysis, limitations and report**.

**Pre-registered comparisons where feasible:** `SafeMovementAgent` vs local novelty; local novelty vs validated FBE; FBE vs FBE+BFS; FBE+BFS vs FBE+BFS+FSM. Testing is conditional on working versions. Record run counts and failure cases; do not claim win-rate advantages before the host reports outcomes reliably.
