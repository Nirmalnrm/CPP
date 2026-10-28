# C++ Capture The Flag Agent

**Joint project:** Software Project (ELT10210) and Autonomous Research Project (KFT11050), West Saxon University of Applied Sciences Zwickau (WHZ)  
**Team:** Aiswarya Payakkal Vinayan and Nirmal Mohan  
**Repository:** [aip25p5h/cpp-agent-project](https://github.fh-zwickau.de/aip25p5h/cpp-agent-project)

## About the project

We are developing a Windows C++ DLL agent for the professor-supplied Capture The Flag graphical host. The host provides a **5 × 5 local world snapshot**, and our DLL returns a movement or flag-interaction action. The final competition uses an initially **unknown map** and an opposing team; our revised research direction is online exploration plus objective navigation rather than known-map pathfinding alone. Software Engineering covers implementation and integration; Research covers algorithm selection, experiment design and comparison.

**Development approach:** iterative and incremental development, with GitHub Issues and lightweight Kanban tracking. The proposed project schedule runs **19 October 2026–31 January 2027**, with **300 planned team person-hours**; January dates are provisional, not a confirmed university deadline.

## Current demonstrated functionality vs planned work

| Stage | Evidence / status |
|---|---|
| Week 1 host + DLL | The `AgentDLLTest` example was compiled and executed; cumulative inputs 10, 20, 30, 40 resulted in 10, 30, 60, 100 (developer diary). |
| Week 2 reference agent | `stupid_agent.cpp` can be built and loaded by the graphical host. Its eight-direction random movement is a baseline. |
| Custom SafeMovementAgent | `safe_agent.cpp` checks four adjacent cardinal cells for known floor and occupancy; the developer diary reports successful host loading and basic tests. The repository source also prefers non-reverse action requests. |
| Mapping, FBE, BFS, FSM and flag capture; optional A* | **Planned/proposed**; not established as working or evaluated in the latest uploaded source. |
| Controlled algorithm experiments | Experimental methodology prepared; quantitative results not yet reported. |

## Documentation

- [Project description](docs/project-description.md) and [requirements](docs/project-requirements.md)
- [Project plan](docs/project-plan.md) — methodology, ownership, scope and risks
- [Project schedule and Gantt chart](docs/project-schedule.md) — 14 work packages, 300 person-hours
- [Software architecture and API](docs/software-architecture.md)
- [Research overview](docs/algorithm-research.md), [research plan](docs/research-plan.md), [literature review](docs/literature-summary.md) and [algorithm comparison](docs/algorithm-comparison.md), and [evaluation metrics](docs/evaluation-metrics.md)
- [developer diary](devlog/developer-diary.md)
## Project layout

```text
cpp-agent-project/
├── README.md
├── docs/                        # definition, plan, schedule, research
│   ├── images/                  # graphical Gantt chart fallback
│   └── presentation/            # editable slides and speaker notes
├── devlog/developer-diary.md
└── tests/
    ├── AgentDLLTest/            # Week 1 interface + host
    └── Week02_HostandAgent/     # Week 2 interface + graphical host + agents
```

## Build and run (Windows / MSYS2 UCRT64)

Use **MSYS2 UCRT64** with MinGW `g++` installed. Run commands from the repository root.

### Week 1 — simple DLL communication test

```bash
cd tests/AgentDLLTest
g++ -std=c++17 -c simple_agent.cpp -o simple_agent.o
g++ -shared -o simple_agent.dll simple_agent.o
./host.exe simple_agent.dll
```

Expected demonstration in the diary: the host sends `10`, `20`, `30`, `40` and receives cumulative results `10`, `30`, `60`, `100`.

### Week 2 — graphical Capture The Flag host

```bash
cd tests/Week02_HostandAgent
g++ -std=c++17 -c stupid_agent.cpp -o stupid_agent.o
g++ -shared -o stupid_agent.dll stupid_agent.o

g++ -std=c++17 -c safe_agent.cpp -o safe_agent.o
g++ -shared -o safe_agent.dll safe_agent.o

./host.exe safe_agent.dll
```

To test the reference agent instead, use `./host.exe stupid_agent.dll`. The supplied Week 2 readme says a second player's DLL can also be provided or dropped onto the host UI. DLL and host must have compatible Windows architectures.


### Week 3 - Project Planning and Research

**Objective:** Establish the project management process, define
the project requirements, prepare the research methodology,
and develop a combined Software Engineering and Research plan.

#### 1. Project Management

- Selected the Iterative and Incremental Development Model.
- Organized project activities using GitHub Issues.
- Defined team responsibilities and review procedures.
- Prepared the initial project plan and schedule.

#### 2. Project Definition and Requirements

- Defined the project problem, objectives, and scope.
- Identified functional and non-functional requirements.
- Documented technical constraints and expected results.
- Defined initial evaluation and acceptance criteria.

#### 3. Team Responsibilities

- **Aiswarya Payakkal Vinayan:** Software development,
  architecture, map memory, BFS, A*, and coordination.
- **Nirmal Mohan:** Research, literature review,
  experimental design, testing, and algorithm evaluation.
- **Both:** Integration, documentation, project review,
  and final presentation.

#### 4. Project Schedule and Gantt Chart

The project is planned from October 2026 to January 2027.

- Total planned effort: **300 person-hours**
- Software Engineering: **110 hours**
- Research and Evaluation: **114 hours**
- Joint activities: **76 hours**
- Total work packages: **14**

The schedule includes milestones, dependencies,
parallel activities, testing, and contingency time.


#### 5. Research Planning

The research investigates navigation and exploration
strategies for the Capture The Flag agent.

Algorithms considered:

- Random Movement (baseline)
- SafeMovementAgent
- Breadth-First Search (BFS)
- A* Search
- Frontier-Based Exploration

Evaluation metrics include blocked movement requests,
repeated movements, navigation efficiency, execution
time, exploration coverage, and flag performance.

BFS, A*, and advanced exploration are planned development
activities, not completed implementations.

#### 6. Research and Software Integration

Software development provides the agent implementations
and testing environment.

Research activities evaluate the algorithms and use
experimental results to guide future improvements.

Main dependencies:

API verification -> Map memory -> BFS/A* ->
Controlled experiments -> Results analysis -> Final report

#### 7. Week 3 Documentation

The following documents describe the project in detail:

- [Project Description](docs/project-description.md)
- [Project Requirements](docs/project-requirements.md)
- [Project Plan](docs/project-plan.md)
- [Project Schedule](docs/project-schedule.md)
- [Software Architecture](docs/software-architecture.md)
- [Research Plan](docs/research-plan.md)
- [Literature Summary](docs/literature-summary.md)
- [Developer Diary](devlog/developer-diary.md)

#### 8. Week 3 Outcome

The initial project definition, requirements,
research methodology, and 300-hour project schedule
have been prepared.

A five-minute presentation focusing on the Gantt chart
and the connection between Software Engineering and
Research was also prepared.

The project plan will be refined based on reviews,
testing results, and supervisor feedback.


### Week 4 — Unknown-Map Competition Research (26–30 October 2026)

**New constraint:** The final competition map is unknown before a match and another team's agent is present. This was reported by the team; specific opponent/flag/scoring rules still require verification.

**Preliminary decision (proposed, not implemented):** investigate a hybrid agent combining obstacle-aware movement, confidence-aware map tracking, novelty/frontier exploration, BFS routes toward known objectives, and visible-opponent response. A* remains an optional comparison.

**Week 4 deliverables:** literature findings and limitations, a justified research choice, requirements/schedule review, five-minute progress presentation, and dated GitHub/diary updates. A working BFS or full competition agent is **not** required for this week's meeting.

**Documents for the revised strategy:**

- [Week 4 competition strategy](docs/week4-competition-strategy.md)
- [Project description](docs/project-description.md)
- [Project requirements](docs/project-requirements.md)
- [300-hour revised schedule / Gantt](docs/project-schedule.md)
- [Research plan](docs/research-plan.md)
- [Algorithm comparison](docs/algorithm-comparison.md)
- [Literature summary](docs/literature-summary.md)
- [Evaluation metrics and protocols](docs/evaluation-metrics.md)
- [Algorithm research index](docs/algorithm-research.md)
- [Software architecture](docs/software-architecture.md)

**Status:** The documents are revised proposals for review. Match outcomes, reliable global mapping, frontier exploration, BFS integration, flag delivery and opponent-aware capabilities have not yet been demonstrated.

### Week 4 refined design: FBE + BFS + FSM (28 October 2026)

**Preliminary choice:** Frontier-Based Exploration (FBE) selects where to explore; BFS plans through **known traversable** cells; a Finite-State Machine (FSM) chooses game objectives and recovery actions. **Safe local exploration or `ACTION_WAIT`** is always available if map/pose confidence is insufficient. **A*** is optional, not a current implementation requirement.

**Two parallel paths:**
- [Software Development Roadmap](docs/software-development-roadmap.md) — Aiswarya: implementation, test gates and integration.
- [Research Roadmap](docs/research-roadmap.md) — Nirmal: literature, hypotheses, baseline/ablation tests, unseen-map evaluation and report.
- [FSM Design](docs/fsm-design.md) — states and guarded transitions.
- [Week 4 Strategy Decision](docs/week4-competition-strategy.md) — rationale, uncertainty and professor questions.

**Project status:** These are **planned** modules; the demonstrated code remains the original reference agent and SafeMovementAgent. No reported win rate, FBE implementation, BFS integration or FSM controller is implied.
