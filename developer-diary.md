# Developer Diary

## Project: Software + Research Project(ELT10210/KFT11050)

**Team Members :-** Aiswarya Payakkal Vinayan , Nirmal Mohan

---

## Week 1 :-

### Task:— Compile and test AgentDLLTest

#### Work performed:

- Compiled `simple_agent.cpp` using the MinGW g++ compiler.
- Generated `simple_agent.dll` from `simple_agent.o`.
- Executed the provided host application using `.\host.exe simple_agent.dll`.
- Verified communication between the host and the agent DLL.

#### Test results:

| Input | Output |
|---|---|
| 10 | 10 |
| 20 | 30 |
| 30 | 60 |
| 40 | 100 |

**Outcome:** The host successfully loaded the agent DLL and all four test calls produced the expected cumulative sums.

---

## Week 2

### Task 1:- Graphical Host and Agent Integration

- Download, compile, link, and test the new graphical host and the provided agent.

#### Activities

We examined Week_02_HostAndAgent.zip, which contains:

- host.exe — graphical Capture The Flag host application.
- agent_api.h — interface between the host and the agent.
- stupid_agent.cpp — example agent implementation.
- readme.txt — compilation and testing instructions.

The new host differs from Week 1 because it provides a graphical game environment in which agents move, encounter obstacles, and can interact with flags.

We used the MSYS2 UCRT64 terminal to compile the example C++ agent into a DLL.

#### Compilation Commands

```bash
g++ -c stupid_agent.cpp -o stupid_agent.o
g++ -shared -o stupid_agent.dll stupid_agent.o
./host.exe stupid_agent.dll
```

#### Observations:-

- The graphical application started, and the original StupidAgent was observed moving through different areas of the game map.
- We subsequently loaded our custom SafeMovementAgent into the graphical host. The host recognized the agent and continued processing its actions.

#### Result:-

**Status:** Successfully demonstrated.

The graphical host-agent interaction works. A completed capture-the-flag game or successful flag capture has not yet been demonstrated.

---

### Task 2 :- API Understanding and Initial Agent Development

- Understand the technical details of the API and develop first agent functionality or helper functions.

#### API Analysis

We studied the following functions:

| Function | Purpose |
|---|---|
| Agent_GetName() | Returns the agent name |
| Agent_Create() | Initializes the agent instance |
| Agent_Reset() | Resets internal state |
| Agent_Step() | Processes an observation and selects an action |
| Agent_Destroy() | Releases allocated memory |

- The host passes the agent a WorldSnapshot containing a 5 × 5 local observation.
- Each cell contains encoded information about terrain and objects. We studied the use of bitmasks to identify floor cells, walls, agents, flags, and targets.

#### Initial SafeMovementAgent

- We created safe_agent.cpp (Used cp command to copy the code exactly form stupid_agent.cpp)
- Unlike StupidAgent, which randomly selects from eight movement directions, SafeMovementAgent checks the four cardinal neighboring cells.

**The initial algorithm:**

- Reads the local snapshot.
- Checks whether neighboring cells are traversable floor.
- Checks whether they contain agents.
- Selects a suitable movement.
- Returns ACTION_WAIT if no suitable cardinal movement is available.

#### Test Results:-

- The custom agent was compiled and loaded into the graphical host.
- The host recognized SafeMovementAgent and executed the simulation.
- However, we observed that the agent frequently moved within the same region instead of exploring larger areas.

#### Problem Analysis :-

- The algorithm uses local movement information but does not remember previously visited locations.
- Consequently, it can repeatedly return to nearby cells.

---

### Task 3:- Start developing the functionality

- Start to develop safe_agent.cpp

**-This version:**

- Checks neighboring cells.
- Rejects known walls and occupied cells.
- Selects randomly among suitable movement options.
- Discourages immediately reversing the previous movement.
- Allows reverse movement when necessary.

**Status:** The improved source is prepared; its successful graphical test still needs to be confirmed.

#### Planned Improvements:-

We plan to investigate movement history, explored-map memory, goal-directed movement, and BFS-based navigation.

---

## Team Contributions:-

### Aiswarya Payyakal Vinayan :-

Demonstrated activities include the Windows DLL tests, graphical host testing.

### Nirmal Mohan :-

Initial SafeMovementAgent development, and investigation of repetitive movement.

### Joint work:

API Analysis, Update Developer Diary, code review, algorithm discussion, Testing, GitHub integration.

## Week 3

## Task 1 — Project Management and Definition

**Work Performed:**
- Created the project description on GitHub.
- Created GitHub Issues to organize project activities.
- Defined project objectives, scope, requirements, and expected outcomes.
- Assigned responsibilities and reviewers for the GitHub Issues.

**Outcome:** Initial project organization and task management established.

## Task 2 — Team Roles and Project Schedule

**Work Performed:**
- Divided responsibilities between Software Engineering and Research.
- Prepared work packages, milestones, and estimated effort.
- Prepared an initial Gantt chart covering both modules.
- Identified parallel activities, dependencies, and time for testing.

**Outcome:** Initial project schedule prepared for team review and presentation.

## Task 3 — Research Planning and Literature Review

**Work Performed:**
- Prepared `research-plan.md` and `literature-summary.md`.
- Identified relevant research papers about autonomous exploration and pathfinding.
- Investigated BFS, A*, random movement, and frontier-based exploration.

**Outcome:** Initial research plan and literature summaries prepared.

## Task 4 — Algorithm Comparison

**Work Performed:**
- Prepared `algorithm-comparison.md`.
- Compared StupidAgent, SafeMovementAgent, BFS, A*, and exploration strategies.

**Outcome:** Algorithm comparison prepared. Experimental evaluation is planned for later development.

## Task 5 — Documentation and Presentation Preparation

**Work Performed:**
- Prepared documentation for the GitHub repository.
- Connected the Software Engineering and Research activities in the project plan.
- Prepared a five-minute presentation with a Gantt chart.
- Divided the presentation topics between both team members.

**Outcome:** Presentation materials prepared for the October 23 meeting.

## Team Contributions

**Aiswarya Payakkal Vinayan:**
- Software Engineering planning.
- Project schedule and Gantt chart preparation.
- Work packages and project management.

**Nirmal Mohan:**
- Research planning and literature review.
- Algorithm comparison.
- GitHub Issue organization.

**Joint Work:**
- Project discussions and documentation.
- GitHub management and reviews.
- Presentation preparation.


---

## Week 4 — Competition Constraints and Project Plan Review (26–30 October 2026)

**Status on 28 October:** Draft change record; confirm which activities were actually performed before marking them complete. The October 30 presentation and supervisor feedback have **not yet occurred** at the time of this entry.

### Updated competition understanding

The team reported that the final match uses a previously unknown map prepared independently and an opposing team's agent. The team therefore proposes shifting the future work plan from standalone BFS/A* development toward safe online exploration, verified map-memory assumptions, BFS routing for known objectives, and opponent-aware decisions where game rules allow.

### Proposed responsibilities / next activities

- **Nirmal — Research:** Record literature findings (Yamauchi, Korf and Sturtevant priority), distinguish published insights from competition hypotheses, prepare unseen-map evaluation metrics and initial test plan.
- **Aiswarya — Software Engineering:** Review revised requirements and 300-hour schedule, validate movement-state assumptions and flag/opponent mechanics, assess the hybrid exploration architecture.
- **Both:** Approve or refine the preliminary strategy, review GitHub Issues and rehearse the October 30 five-minute progress presentation.

### Changes proposed for review

Revised project description, requirements, plan, schedule/Gantt, research plan, algorithm comparison and software architecture. Added a competition research decision and measurable evaluation protocol. The original 14 work-package IDs, estimated 300 person-hours, team ownership and proposed date ranges are retained.

### Open questions

Movement success detection; diagonal legality; flag handling and match winner feedback; opponent interaction rules; permitted maps/seeds for controlled comparisons; feasibility of reliable global map memory.

### To update after October 30 meeting

- **Actual tasks completed and person-hours:** [Fill from work performed]
- **Review decisions by both teammates:** [Fill after review]
- **Supervisor questions/feedback:** [Fill after meeting]
- **Git commit hashes and test evidence:** [Fill after committing and running tests]

### Week 4 — Refinement of proposed algorithm structure (28 October 2026)

**Design draft, awaiting teammate agreement:** The proposed competition approach is now described explicitly as **Frontier-Based Exploration (FBE) + BFS + Finite-State Machine (FSM)** with a **safe local fallback**; A* is optional. FBE chooses unknown regions, BFS follows routes through known cells, and the FSM organizes explore/flag/return/opponent/recovery behavior. This is a documentation and preliminary research decision, **not an implementation or experimental result**.

**Next ownership checks:** Aiswarya reviews movement/pose handling, FBE/BFS/FSM feasibility and the updated Gantt; Nirmal reviews the literature basis, research questions, test protocol, ablations and held-out evaluation maps.

**Actual implementation, tests, review comments and October 30 feedback:** [Record only after completed.]
