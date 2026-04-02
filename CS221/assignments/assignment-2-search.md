# CS221 Assignment 2: Search

## Problem Statement
This is the assignment where route search stops being a toy and starts looking like Stanford's map problem. The CS221 Autumn 2025 lecture repo splits the material into `search` and `ucs_astar`, and the route-planning homework repo describes the actual task as shortest paths on Stanford campus, plus unordered waypoints and A* speedups. See [`autumn2025-lectures`](https://github.com/stanford-cs221/autumn2025-lectures) and [`Route-Planning`](https://github.com/AhmedAbdelaal2001/Route-Planning).

BFS is the natural baseline in your head. The assignment pushes past that quickly: once step costs differ, frontier order alone is not enough.

## Key Techniques
- Graph search with an explicit frontier.
- Path cost accumulation for UCS.
- Heuristic search for A*.
- State design that carries both location and remaining waypoints.
- Straight-line and no-waypoints heuristics as domain knowledge, not magic.

## Implementation Walkthrough
- Model the problem first. The route-planning repo describes `ShortestPathProblem` for point-to-point routing and `WaypointsShortestPathProblem` when stops are unordered.
- Run UCS to get optimal routes when costs matter.
- Switch to A* once you have a useful heuristic. The repo's README calls out `StraightLineHeuristic` and `NoWaypointsHeuristic` as the speed path.
- For waypoint problems, make the state include both current node and the set of unmet stops. That is the whole trick.
- The algorithm stays simple. The state representation does the heavy lifting.

## What You Learn
- BFS is the "uniform cost" intuition; UCS is the general version.
- A* is only fast when the heuristic is doing real work.
- Search performance is mostly a state-modeling problem.
- Good heuristics compress map knowledge into one number per state.
