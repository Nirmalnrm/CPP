# EXACT FILE CHANGES — C++ CTF Project (6) ZIP

**Source reviewed:** `cpp-agent-project(6).zip` (current repository snapshot, latest file edits dated October 2026).  
**New preliminary research/software direction:** **Frontier-Based Exploration (FBE) + BFS + Finite-State Machine (FSM)**, with a **safe local fallback**; **A* optional**.  
**Revision planning date:** 29 October 2026.  
**Schedule:** 14 work packages, **300 planned person-hours**, **19 October 2026 – 31 January 2027**.  
**Do not claim** this hybrid agent has been implemented or that experiments have been completed.

## A. What the uploaded ZIP currently contains

The ZIP contains Week 1 DLL testing, Week 2 StupidAgent and SafeMovementAgent, and substantive Week 3 planning/research. However:

- `docs/project-schedule.md` and `docs/Gantt_Chart.xlsx` still have **WP-04 BFS navigation** and **WP-05 A* navigation/flag behavior**.
- `docs/project-plan.md`, `docs/research-plan.md`, `docs/algorithm-comparison.md`, and `docs/software-architecture.md` still prioritize the older BFS/A* development sequence.
- `docs/algorithm-research.md` is **empty** (0 bytes).
- `docs/evaluation-metrics.md` is **missing**, even though existing documents link to it.
- `README.md` and `devlog/developer-diary.md` end at Week 3.
- The ZIP does **not** include completed hybrid FBE/BFS/FSM C++ code. The revised documents correctly call these components **planned**.

## B. Exact changes — files you replace at the same paths

The replacement files in this package already contain the new wording. **Do not paste the whole sections manually** unless you prefer to edit in the GitHub browser. For each file below, copy the corresponding replacement from this ZIP into the repository at the same location.

| Path | Exact sections / differences | What must remain true |
|---|---|---|
| `docs/project-description.md` | Add previously unknown competition map, opponent agent, and hybrid FBE+BFS+FSM approach | Host DLL still provided by professor; not a finished hybrid agent |
| `docs/project-requirements.md` | Refine **FR-07–FR-12** for objective detection, unseen maps, safe recovery and opponents; add **FR-13 FSM** and **FR-14 frontier safety**, plus **NFR-08 generalization** and validation checks | Preserve FR-01–FR-06, existing API and earlier tests |
| `docs/project-plan.md` | Update **§2 objectives**, **§3 scope**, **§6 WP titles/outputs**, **§7 dependencies**, **§8 research questions**, **§9 architecture direction**, **§10 test criteria**, **§11 risks**; add **§13 separate development/research roadmaps** | Keep iterative model, original teammates, 14 WP IDs and 300 h |
| `docs/project-schedule.md` | Replace old WP names with FBE/FSM plan in **Detailed work packages**, **Planned outputs**, **Dependencies**, **Gantt Chart**. Add Oct 30 progress milestone and roadmap links | Preserve existing start/end dates, workload owners, 14 WP IDs, 300 h |
| `docs/Gantt_Chart.xlsx` | Updated editable Gantt with new titles, dependencies and **Software Path**, **Research Path**, **Competition Research** sheets | The 14 bars, durations, October–January horizon and 300 h remain consistent |
| `docs/research-plan.md` | Revise **§1 background**, **§2 questions + RQ6 on FSM**, **§4 algorithm priorities**, **§5 controlled experimental phases**, **§6 metrics**, **§7 integration**, **§8 risk/uncertainty** and add **§9 research path** | Keep the six scholarly references; no invented experimental results |
| `docs/algorithm-comparison.md` | Include FSM as a **behavior controller**, not a pathfinder; expand trade-offs; revise **§7 recommended decision/order** to FBE+BFS+FSM with local fallback; A* optional | Preserve original tested random/safe baselines and comparisons |
| `docs/literature-summary.md` | Append/revise Week 4 synthesis and source limitations for unknown-map exploration, online decisions and fair benchmarking | Retain original six paper citations and their actual review depth |
| `docs/software-architecture.md` | Modify **§5 planned modules** and **§6 future agent decision flow** to include movement confidence, map, frontier selection, known-route BFS, FSM states, recovery and opponent response | Keep current host/API/implemented sections clear and truthful |
| `README.md` | Preserve Week 1–3 sections; append Week 4 unknown-map competition decision and links to roadmaps/FSM/evaluation docs; fix missing-doc links | Do not overwrite build steps or claim unimplemented code works |
| `devlog/developer-diary.md` | Preserve Weeks 1–3; append Week 4 **proposed design decisions and planned tasks** | Before submitting, correct dates/contributors to match actual work; add Oct 30 professor feedback afterward |

## C. New files to add

| Path | Why it is needed |
|---|---|
| `docs/algorithm-research.md` | Fills the empty research-index placeholder with links to the real research documents |
| `docs/evaluation-metrics.md` | Provides observable movement, objective, reliability and later win-rate measures with a fair testing protocol |
| `docs/week4-competition-strategy.md` | Records the **preliminary** FBE+BFS+FSM choice, assumptions and questions for Professor Hipp |
| `docs/software-development-roadmap.md` | **Aiswarya's path:** API verification → safe fallback → position confidence → valid frontiers → BFS → FSM/flag response → QA; A* optional |
| `docs/research-roadmap.md` | **Nirmal's path:** literature/hypotheses → measurement rules → fair baselines → test components → held-out maps/opponents → analysis/report |
| `docs/fsm-design.md` | Proposed FSM states, transitions, priority rules, uncertainty handling, and synthetic tests |
| `docs/week4-github-issues.md` | Draft GitHub Issue descriptions; **not posted** to university GitHub |
| `docs/week4-presentation-replacements.md` | Text for five updated progress slides; **not an edited PPTX** |

## D. Change the Gantt chart — exact old and new titles

These are the **new** work-package names in both `docs/project-schedule.md` and `docs/Gantt_Chart.xlsx`.

| WP | Old title in your uploaded ZIP | New title | Hours | Owner |
|---|---|---|---:|---|
| 01 | Requirements & project definition | Competition requirements & rules | 12 | Aiswarya |
| 02 | Architecture & API verification | API, movement outcome & pose checks | 14 | Aiswarya |
| 03 | Map memory & exploration logic | Map memory & confidence tracking | 30 | Aiswarya |
| 04 | BFS navigation prototype | Frontier exploration & safe fallback | 24 | Aiswarya |
| 05 | A* navigation & flag behavior | FSM, BFS, flags & opponent response | 30 | Aiswarya |
| 06 | Literature review & hypotheses | Unknown-map literature & hypotheses | 24 | Nirmal |
| 07 | Metrics & experimental design | Competition metrics & experimental design | 18 | Nirmal |
| 08 | Test harness & baseline runs | Held-out maps, harness & baselines | 24 | Nirmal |
| 09 | Algorithm comparison experiments | Tournament tests & FSM/FBE ablations | 28 | Nirmal |
| 10 | Results analysis & validity | Win-rate, robustness & validity analysis | 20 | Nirmal |
| 11 | Schedule, review & presentation | Schedule, review & competition briefing | 12 | Both |
| 12 | Integration, debugging & QA | Agent integration, opponent tests & QA | 34 | Both |
| 13 | Final documentation & report | Final documentation & research report | 20 | Both |
| 14 | Contingency & final review | Contingency & competition readiness | 10 | Both |

**Budget check:** Software 110 h + Research 114 h + Joint 76 h = **300 h**. Shared work is allocated equally: **Aiswarya 148 h; Nirmal 152 h**. These are **estimates, not completed work**.

**Dependencies:** Validate host movement and state → maintain reliable map where feasible → explore via frontiers with safe fallback → BFS across known routes → FSM manages objectives/opponents/recovery → controlled comparisons. Research baseline and literature work proceed concurrently. WP-04 and WP-05 intentionally overlap late WP-03; only enable mapping-dependent behaviors after validation.

## E. Files you DO NOT rewrite

- Everything inside `tests/AgentDLLTest/` (Week 1 baseline).
- `tests/Week02_HostandAgent/stupid_agent.cpp` and `safe_agent.cpp` (reference baselines).
- Professor-provided Week 2 `agent_api.h`, `host.exe`, and API signatures.
- Existing Git commits or the `.git` directory.
- Actual Week 1–3 diary history. Add a new Week 4 section rather than rewriting past accomplishments.

The ZIP is a **documentation/scheduling update**, not implementation of the competition strategy. When ready, add a new versioned C++ source for the hybrid strategy and test it separately.

## F. Safe steps in Windows MSYS2 UCRT64

1. Close Excel if the Gantt file is open. Back up or commit local/uncommitted teammate changes first.
2. Extract this package **into the repository root** (same folder as `README.md`). Merge `docs/` and `devlog/`; allow replacement **only after reviewing differences**.
3. Verify the 14 titles, the 300 h total, and the links in `docs/project-schedule.md`.
4. Copy Week 4 issue descriptions into the university GitHub Issues UI; they are not already created.
5. Use the following commands (adjust path if your local clone differs):

```bash
cd /c/Users/nirma/class/cpp/cpp-agent-project

git status
git remote -v
git diff --stat
git diff -- docs/project-plan.md docs/research-plan.md docs/project-schedule.md

git add README.md docs/ devlog/developer-diary.md
git diff --cached --check
git diff --cached --stat
git commit -m "Revise software and research plans for FBE BFS FSM"
git push origin main
```

**Before pushing:** confirm `origin` is the team's intended shared university repository. Do not force-push. If another contributor changed a document after this snapshot, merge/review instead of blindly replacing it.

## G. What is actually required for Week 4

Professor Hipp's October 30 meeting calls for **literature findings, preliminary approach selection, review of the project definition/schedule, a five-minute progress presentation, and repository/diary updates**. You do **not** need a fully implemented frontier/BFS/FSM competition agent by that meeting.

**Open API questions:** How is movement success confirmed? How does the host expose flag possession or match results? Are diagonal corner moves legal? What do enemy interaction rules allow? How can test maps be repeated for a controlled experiment?
