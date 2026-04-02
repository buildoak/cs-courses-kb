# CS149 Assignment 5: Big Graph Processing in OpenMP

## Problem Statement

`biggraphs-ec` is an extra-credit assignment, but the workload is not extra-credit-sized. The goal is to run breadth-first search on graphs with tens or hundreds of millions of edges using OpenMP on a multicore CPU. The repo gives students a correct sequential top-down BFS, plus empty hooks for bottom-up and hybrid BFS, and then grades both correctness and performance on large datasets.

What makes the assignment interesting is that it is not "parallelize BFS" in the generic sense. It is "parallelize three different frontier traversal strategies, understand when each one wins, and switch between them at runtime." The top-down version pushes work outward from the current frontier. The bottom-up version asks every unvisited node whether any parent is already in the frontier. The hybrid version exists because the best answer changes during the traversal.

## Key Techniques

- **Compressed graph traversal**: work directly with the starter graph representation, where outgoing and incoming edge arrays are stored contiguously and indexed by `*_starts`.
- **Frontier-based BFS**: represent the active wavefront explicitly with `vertex_set`, then build the next frontier each step.
- **Compare-and-swap for visitation**: use atomic claim-once updates for `distances[]` so multiple threads can race to discover the same vertex without duplicate insertion.
- **Frontier bitmaps or boolean membership arrays**: make bottom-up BFS fast by turning "is parent in frontier?" into an O(1) membership test.
- **Direction-optimizing BFS**: switch between top-down and bottom-up based on frontier size or estimated edge work.
- **OpenMP scheduling and local buffering**: reduce contention by using per-thread scratch space or chunked appends instead of a single critical section around the frontier.

## Implementation Walkthrough

1. **Start from the provided sequential top-down code.** `bfs/bfs.cpp` already includes `top_down_step(...)` and `bfs_top_down(...)`. The code walks every vertex in the frontier, iterates over its outgoing edges, and appends newly discovered neighbors into `new_frontier`. That baseline also makes the main shared state obvious: `distances[]` and the next-frontier append pointer.
2. **Parallelize top-down without a giant lock.** The README explicitly points students toward `__sync_bool_compare_and_swap` instead of `#pragma omp critical`. A standard solution is to parallelize over frontier vertices, inspect outgoing neighbors, and use compare-and-swap to claim an unvisited neighbor by changing its distance from `NOT_VISITED_MARKER` to `distances[node] + 1`. Once the claim succeeds, the thread appends the vertex to the next frontier via an atomic counter or a thread-local buffer that is merged later.
3. **Implement bottom-up BFS as a frontier-membership query.** The empty `bfs_bottom_up(...)` is where the algorithm flips. Instead of iterating outward from frontier vertices, iterate over all unvisited vertices and scan their incoming edges. The moment one incoming neighbor is in the current frontier, mark the vertex visited, record its distance, and add it to the next frontier. This works best when frontier membership is represented as a bitmap or boolean array, because scanning a whole `vertex_set` for every incoming edge would destroy performance.
4. **Build the hybrid traversal around representation conversion.** `bfs_hybrid(...)` is also empty in the starter. The practical design is: keep both a list-style frontier for top-down work distribution and a membership structure for bottom-up tests, then choose the traversal mode each level using a heuristic based on current frontier size, number of unexplored edges, or observed runtime. The cost of converting between list and bitmap representations is part of the optimization problem, not an implementation detail to ignore.
5. **Use the repo structure as a performance guide.** `common/graph.h` exposes both outgoing and incoming adjacency arrays, which is exactly what the two traversal directions need. `bfs/bfs.h` defines the minimal `vertex_set` container students are expected to extend or work around. `bfs/main.cpp` and `bfs/grade.cpp` make clear that correctness is checked separately for top-down, bottom-up, and hybrid implementations.
6. **Benchmark on the right machine and the right graphs.** The README says grading happens on Myth machines and points students at both real-world graphs like `soc-livejournal1_68m.graph` and synthetic graphs like `rmat_200m.graph`. The important practical lesson is that graph structure changes which BFS direction is cheaper, so a hybrid heuristic that works on one graph family may be weak on another.

## What You Learn

- BFS is not one algorithm at scale. It is a family of traversal directions with different synchronization and locality profiles.
- Atomic compare-and-swap is often the right primitive when many threads may discover the same vertex concurrently.
- The best frontier representation depends on the traversal direction; list and bitmap views both matter.
- Hybrid algorithms are worthwhile only when the switch heuristic is cheaper than staying in the wrong mode.
- Large-graph performance is shaped by contention, memory bandwidth, and graph structure long before it is shaped by raw core count.

## Sources

- Assignment handout: `https://github.com/stanford-cs149/biggraphs-ec/blob/master/README.md`
- BFS starter code: `https://github.com/stanford-cs149/biggraphs-ec/blob/master/bfs/bfs.cpp`
- BFS headers: `https://github.com/stanford-cs149/biggraphs-ec/blob/master/bfs/bfs.h`
- Graph representation: `https://github.com/stanford-cs149/biggraphs-ec/blob/master/common/graph.h`
- BFS driver and grader: `https://github.com/stanford-cs149/biggraphs-ec/blob/master/bfs/main.cpp`, `https://github.com/stanford-cs149/biggraphs-ec/blob/master/bfs/grade.cpp`
- GitHub repo: `https://github.com/stanford-cs149/biggraphs-ec`
