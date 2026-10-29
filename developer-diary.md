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


## Week 4 — 29 October 2026

### Task: Research decision and preparation for the October 30 meeting

**Evidence reviewed:** repository source, API header, initial literature survey, research plan, revised requirements/architecture, schedule and supplied Gantt workbook.

**Progress recorded:**
- The documents propose FBE + BFS + FSM with a safe local fallback for the unknown-map competition. Mapping remains gated on reliable movement inference.
- Literature summaries distinguish initial findings from untested hypotheses. Research-question references now follow the current RQ1–RQ5 definitions.
- Requirements, work packages and open questions cover localization, flag interactions, opponents, measurement feasibility and the provisional final deadline.
- Meeting preparation adds a five-minute presentation and timed speaker notes. Delivery and rehearsal are not recorded as completed.
- The Gantt responsibility-summary formulas are corrected. WP-01, WP-02, WP-06, WP-07 and WP-11 are In progress based on available evidence. Other packages remain Planned.
- The research overview and evaluation-metrics index are populated, and the README links to the meeting materials and workbook.

### Individual contributions and traceability

| Person | Contribution supported by the existing diary | Week 4 responsibility / review still to perform |
|---|---|---|
| Aiswarya Payakkal Vinayan | DLL/graphical-host testing; software planning, schedule and work packages | Review software status and schedule; present implementation progress and supervisor questions |
| Nirmal Mohan | Initial safe-agent development, research planning, literature review, algorithm comparison and issue organization | Review literature interpretation and protocol; present findings and evaluation plan |
| Both | API discussion, documentation, GitHub coordination and presentation preparation | Approve the preliminary strategy, rehearse and record supervisor feedback |

The archived Git history includes October 29 document changes under account `nim25p36`. Account history alone does not establish who performed each research activity. This entry records documented contributions and assigned responsibilities without inventing hours or individual Week 4 tests. The meeting pack and consistency corrections were prepared with AI assistance from the uploaded materials; team review of those updates is pending.

### Software evidence and limitations

The current safe source filters known floor/occupancy among four cardinal neighbors, disfavors immediate reverse requests, and waits when blocked. A Linux C++17 syntax check passed during the document review. This does not establish Windows DLL linkage or host behavior. The diary's earlier host demonstration concerns the initial safe version. The current improved version still needs a recorded Windows graphical-host test. FBE, BFS, FSM, persistent spatial memory, flag handling and quantitative comparison remain unimplemented or unverified.

### Decision and next steps

Use the hybrid as the preliminary investigation direction, with A* optional. Verify movement outcomes and host rules, implement safe local exploration, then attempt confidence-aware mapping/frontiers and BFS/FSM integration. Research will compare working variants on matched development and held-out maps. Direct FSM and opponent-policy reading, measurement feasibility and trial counts remain open.

See [Week 4 decision and status](../docs/week4-status.md), [presentation notes](../docs/presentation/week4-speaker-notes.md), and [updated Gantt](../docs/planning/Gantt_Chart_Week4.xlsx). Commit history records the preparation changes. A local commit or archived remote-tracking reference is not proof of a successful push.
