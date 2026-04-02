# 15-445 Homework 3: Join Algorithms

## Problem Statement
The join section of Homework 3 is a page-cost drill. You are given table sizes, buffer sizes, and a fixed I/O model, then asked to compare simple nested loop join, block nested loop join, sort-merge join, hash join, and join-order choices.

The worksheet is built to make one point: join strategy is an I/O problem first. Memory budget, relation size, and duplicate behavior decide the answer before any code does.

## Key Techniques
- Choose the outer relation carefully. In nested loop joins, table size can turn a bad plan into a worse one.
- Use block nested loop join when you can amortize inner scans over a chunk of outer pages.
- Sort-merge join depends on sort cost plus merge cost, and the merge cost changes when duplicates explode.
- Hash join is partition-and-probe under a memory budget, not magic.
- Index nested loop join only wins when an index actually exists on the join key.
- Read the true/false questions as optimizer heuristics: they are testing whether you understand when each join shape is a trap.

## Implementation Walkthrough
1. The plan layer names the join shape explicitly. `NestedLoopJoinPlanNode`, `HashJoinPlanNode`, and `NestedIndexJoinPlanNode` each carry the child plans and the join keys or predicate.
2. `ExecutorFactory::CreateExecutor` is the dispatch point. It routes those plan nodes to `NestedLoopJoinExecutor`, `HashJoinExecutor`, or `NestedIndexJoinExecutor`.
3. Each executor is built around the same pattern: initialize children, buffer the tuples you need, then emit joined batches through `Next(...)`.
4. The current codebase still has the join executors marked `TODO(P3)`, which makes the homework mirror cleanly into the implementation gap.
5. The cost math from the worksheet is the same decision tree the executor implementation has to respect: nested loops for brute-force correctness, hash join for partitioned equality joins, and index joins when the inner side already has an index.

## What You Learn
- Join choice is about page traffic, not just asymptotic notation.
- The "best" join depends on which side is smaller and which structure already exists.
- Duplicate-heavy data changes sort-merge cost in a way that surprises people the first time.
- Hash joins and nested loops are different answers to the same memory constraint.
- A cost model is useful because it tells you when the obvious plan is the wrong one.

## Sources
- `https://15445.courses.cs.cmu.edu/fall2023/files/hw3-clean.pdf` (accessed 2026-04-02)
- `https://15445.courses.cs.cmu.edu/fall2023/files/hw3-clean.pdf` extracted with `summarize --extract --plain` (accessed 2026-04-02)
- `/Users/otonashi/thinking/tmp/bustub/src/include/execution/plans/nested_loop_join_plan.h:27-73`
- `/Users/otonashi/thinking/tmp/bustub/src/include/execution/plans/hash_join_plan.h:25-80`
- `/Users/otonashi/thinking/tmp/bustub/src/include/execution/plans/nested_index_join_plan.h:29-87`
- `/Users/otonashi/thinking/tmp/bustub/src/execution/executor_factory.cpp:111-141`
- `/Users/otonashi/thinking/tmp/bustub/src/execution/nested_loop_join_executor.cpp:20-50`
- `/Users/otonashi/thinking/tmp/bustub/src/execution/hash_join_executor.cpp:18-48`
- `/Users/otonashi/thinking/tmp/bustub/src/execution/nested_index_join_executor.cpp:18-38`
