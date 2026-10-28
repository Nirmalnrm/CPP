# Apply these revised files — FBE + BFS + FSM

**Source:** `cpp-agent-project(6).zip`. **Status:** Proposed Week 4 planning updates, not an implemented winning agent.

## The 10 requested text files

| File | What was changed |
|---|---|
| `docs/project-description.md` | Defines unknown-map competition, proposes FBE+BFS+FSM with safe fallback and optional A*; separates software and research. |
| `docs/project-requirements.md` | Updates future functionality, safe recovery, FSM transitions, map confidence, competition generalization and evaluation acceptance criteria. |
| `docs/project-plan.md` | Revises objectives, scope, 14 work packages, 300-hour allocations, dependencies, risks and integrated research strategy. |
| `docs/project-schedule.md` | Revises work-package names, milestones, dependencies and the Mermaid Gantt chart (Oct 2026–Jan 2027). |
| `docs/research-plan.md` | Research hypotheses, unexplored-map trials, controlled comparisons including an FSM ablation, reliable metrics and study limits. |
| `docs/algorithm-comparison.md` | FBE explores, BFS plans known routes, FSM selects objectives; fallback when mapping uncertain; A* optional. |
| `docs/literature-summary.md` | Preserves six original sources, annotates Week 4 implications, identifies limits of using them as evidence for FSM. |
| `docs/software-architecture.md` | Proposes FBE, BFS, FSM, map-confidence tracker and safe recovery without altering the five exports. |
| `README.md` | Preserves Week 1–3 status and appends Week 4 navigation, links and explicit planned/not-implemented labeling. |
| `devlog/developer-diary.md` | Preserves historical records and adds a Week 4 **draft** for the team to update with actual dates, work and feedback. |

## Companion files in the COMPLETE update ZIP

`docs/Gantt_Chart.xlsx` is the editable 300-hour schedule, aligned with the Mermaid chart. Other `docs/*.md` files supply the referenced evaluation-metrics protocol, research and software development paths, proposed FSM design, competition decision, and Week 4 issue/presentation drafts.

**If you use only the ten-file ZIP**, keep the companion docs already in your repository or use the complete ZIP to avoid missing Markdown links.

## Safe application steps

1. Commit/back up any existing local changes and pull the latest collaborator edits.
2. Extract the **complete** package into the Git repository root so `README.md`, `docs/` and `devlog/` have the same relative paths. Review incoming diffs before replacement.
3. Correct the Week 4 developer-diary draft to reflect actual completed work. Do not mark the October 30 presentation/feedback complete prematurely.
4. Do **not** replace `agent_api.h`, the graphical host, Week 1 test source, `stupid_agent.cpp`, `safe_agent.cpp` or `.git/`.
5. Inspect the diff and check your remote before committing:

```bash
cd /c/Users/nirma/class/cpp/cpp-agent-project
git status
git remote -v
git diff -- README.md docs/ devlog/developer-diary.md
git add README.md docs/ devlog/developer-diary.md
git diff --cached --check
git diff --cached --stat
git commit -m "Update competition software and research plans for FBE BFS FSM"
git push origin main
```

Verify your shared university GitHub remote and branch name; do not force-push.

## Planned development and research paths

**Software (Aiswarya):** Verify host API/outcomes → safe local fallback → map and pose confidence → reachable frontier selection → BFS on known cells → FSM + verified flag/opponent rules → integration and QA → optional A*.

**Research (Nirmal):** Literature + initial findings → hypotheses and measurement limits → repeatable baselines → component unit tests → held-out unknown maps / varied opponents → FBE, BFS and FSM ablation comparisons → validity/robustness analysis and final report.

**Estimates:** Software 110 h + Research 114 h + Joint 76 h = **300 h** across 14 work packages. With shared effort split equally: Aiswarya 148 h, Nirmal 152 h.
