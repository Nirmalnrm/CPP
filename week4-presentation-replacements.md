# Week 4 Presentation — Replacement Slide Text

**Meeting:** Friday, 30 October 2026  
**Total:** 5 minutes, shared between Aiswarya and Nirmal  
**Status:** Proposed slide content for the presentation already prepared by the team. Delivering the talk and receiving feedback remain pending.

## Slide 1 — Software Progress (Aiswarya, 35 seconds)

**Title:** Current Status: Working Baselines

- C++ DLL integrated with the professor's graphical host.
- Two baselines: `StupidAgent` and obstacle-aware `SafeMovementAgent`.
- API provides a **5 × 5 local snapshot**, not global coordinates.
- Repetitive movement remains an identified limitation.
- **Competition change:** the final map is unknown until the match, with an opposing team's agent.

## Slide 2 — Literature Findings (Nirmal, 70 seconds)

**Title:** Initial Research: Exploration and Evaluation

- **Yamauchi (1997):** frontier exploration can seek the boundary of explored and unexplored space; requires a reliable map.
- **Korf (1990):** real-time planning updates decisions as observations arrive.
- **Sturtevant (2012):** comparisons should use consistent scenarios and multiple grid layouts.
- **Finding:** BFS and A* plan paths to known goals but do not independently discover an unknown flag.
- **Limitation:** no cited paper proves which algorithm wins this exact game.

## Slide 3 — Preliminary Approach (Nirmal, 75 seconds)

**Title:** Frontier + BFS + FSM with Safe Fallback

1. Decode and filter known blocked moves.
2. Verify displacement; preserve confidence in map state.
3. **Frontier-Based Exploration (FBE)** selects where to search; requires trusted map information.
4. **BFS** finds a route through known cells toward a frontier, flag or target.
5. **Finite-State Machine (FSM)** chooses explore / approach / pickup / return / deliver / evade / recover using verified observations.
6. **Safe local exploration** or wait is the fallback if localization or planning is uncertain; **A* optional**.

**Status:** Research proposal only, not an implemented or benchmarked competition agent.

## Slide 4 — Project Plan and Gantt (Aiswarya, 60 seconds)

**Title:** Updated Plan: 14 Work Packages, 300 Hours

- WP-02: movement-success and API verification.
- WP-03: map memory and confidence.
- WP-04: online/frontier exploration and safe fallback.
- WP-05: BFS + FSM for known-goal navigation, flag actions and visible-opponent response.
- WP-08–10: held-out map tests, opponent comparisons, FSM/FBE ablations and validity analysis.
- **300 total planned person-hours; 148 Aiswarya and 152 Nirmal including joint activities.**
- Dates and effort remain provisional; not yet hours worked.

## Slide 5 — Next Steps and Questions (Both, 60 seconds)

**Title:** Next Steps and Questions for Professor Hipp

- Investigate last-action success and legal diagonal rules.
- Confirm pickup/delivery and game-winner feedback.
- Create repeatable baseline tests with different map layouts and opponents.
- Keep held-out maps separate from development data; do not tune on the secret competition map.
- Update GitHub Issues, research findings and developer diary.

**Question:** Which game events are observable through the host, and how can we repeat comparable matches?

---

**Total speaking time:** 35 + 70 + 75 + 60 + 60 = **300 seconds**. Practice aloud; the slides are intended to support the presenters, not be read word for word.
