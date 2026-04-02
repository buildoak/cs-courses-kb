# 15-445 Project 3: Query Execution

## Problem Statement
Project 3 turns a plan tree into execution. The input is already an `AbstractPlanNodeRef`, and the remaining job is to execute the tree correctly and in order. The executor layer has to support scans, inserts, updates, deletes, joins, aggregation, sorting, top-N, and window functions without breaking the iterator contract. [/Users/otonashi/thinking/tmp/bustub/src/include/execution/plans/abstract_plan.h:33-64] [/Users/otonashi/thinking/tmp/bustub/src/include/execution/executors/abstract_executor.h:22-54]

## Key Techniques
- Think in plan trees and executor trees. `PlanType` defines the operator surface, and each plan node carries child plans plus output schema. [/Users/otonashi/thinking/tmp/bustub/src/include/execution/plans/abstract_plan.h:33-64]
- Treat `AbstractExecutor` as the Volcano contract. Executors must `Init()` before `Next()`, and `Next()` produces tuple batches plus RID batches. [/Users/otonashi/thinking/tmp/bustub/src/include/execution/executors/abstract_executor.h:22-54]
- Use the factory as the wiring layer. `ExecutorFactory::CreateExecutor` maps each plan type to the correct executor and nests child executors where needed. [/Users/otonashi/thinking/tmp/bustub/src/execution/executor_factory.cpp:50-199]
- Keep sort logic in shared helpers. `TupleComparator` and `GenerateSortKey()` are the P3-specific utilities behind `Sort` and `TopN`. [/Users/otonashi/thinking/tmp/bustub/src/include/execution/execution_common.h:27-47] [/Users/otonashi/thinking/tmp/bustub/src/execution/execution_common.cpp:23-34]
- Let the engine own the polling loop. `ExecutionEngine::Execute()` creates the root executor, calls `Init()`, drains it in batches, and clears partial output if an `ExecutionException` escapes. [/Users/otonashi/thinking/tmp/bustub/src/include/execution/execution_engine.h:29-85]

## Implementation Walkthrough
1. Start with the plan vocabulary. BusTub exposes `SeqScan`, `IndexScan`, `Insert`, `Update`, `Delete`, `Aggregation`, `Limit`, `NestedLoopJoin`, `NestedIndexJoin`, `HashJoin`, `Filter`, `Values`, `Projection`, `Sort`, `TopN`, `TopNPerGroup`, `MockScan`, `InitCheck`, and `Window`. The executor layer mirrors that list. [/Users/otonashi/thinking/tmp/bustub/src/include/execution/plans/abstract_plan.h:33-54]
2. Wire `ExecutorFactory` first. The factory is where child executors get constructed, where nested-loop joins can be wrapped in `InitCheckExecutor`, and where `Sort` becomes `ExternalMergeSortExecutor<2>`. That is the shape of the whole project in one switch statement. [/Users/otonashi/thinking/tmp/bustub/src/execution/executor_factory.cpp:56-195]
3. Implement the simple operators before the compound ones. Seq scan, index scan, values, projection, filter, insert, update, delete, and limit all share the same basic pattern: hold child state, initialize once, and emit tuples from `Next()`. [/Users/otonashi/thinking/tmp/bustub/src/execution/executor_factory.cpp:60-96] [/Users/otonashi/thinking/tmp/bustub/src/execution/executor_factory.cpp:144-168]
4. Add the stateful operators next. Aggregation, joins, sort, top-N, top-N-per-group, and window functions all need internal buffering or hashing, which is why they sit behind separate executor classes and helper code. [/Users/otonashi/thinking/tmp/bustub/src/execution/executor_factory.cpp:98-195]
5. Keep the engine loop dumb. `ExecutionEngine::PollExecutor()` just keeps calling `Next()` until exhaustion, which means every executor has to honor its own end-of-stream semantics cleanly. [/Users/otonashi/thinking/tmp/bustub/src/include/execution/execution_engine.h:88-104]
6. Use the tests as an operator map. The `p3.*.slt` files cover seqscan, insert, update, delete, B+Tree index scan, aggregation, joins, sort and limit, top-N, window functions, composite-key index scans, and integration workloads. That is the real scope of the assignment. [/Users/otonashi/thinking/tmp/bustub/test/sql/p3.01-seqscan.slt] [/Users/otonashi/thinking/tmp/bustub/test/sql/p3.02-insert.slt] [/Users/otonashi/thinking/tmp/bustub/test/sql/p3.03-update.slt] [/Users/otonashi/thinking/tmp/bustub/test/sql/p3.04-delete.slt] [/Users/otonashi/thinking/tmp/bustub/test/sql/p3.05-index-scan-btree.slt] [/Users/otonashi/thinking/tmp/bustub/test/sql/p3.07-simple-agg.slt] [/Users/otonashi/thinking/tmp/bustub/test/sql/p3.10-simple-join.slt] [/Users/otonashi/thinking/tmp/bustub/test/sql/p3.14-hash-join.slt] [/Users/otonashi/thinking/tmp/bustub/test/sql/p3.16-sort-limit.slt] [/Users/otonashi/thinking/tmp/bustub/test/sql/p3.17-topn.slt] [/Users/otonashi/thinking/tmp/bustub/test/sql/p3.20-window-function.slt] [/Users/otonashi/thinking/tmp/bustub/test/sql/p3.22-composite-key-index-scan.slt] [/Users/otonashi/thinking/tmp/bustub/test/sql/p3.18-integration-1.slt] [/Users/otonashi/thinking/tmp/bustub/test/sql/p3.19-integration-2.slt]

## What You Learn
- Query execution is composition. Each executor is small, but the tree of executors is the real machine.
- The factory pattern matters because the plan tree already encodes the execution shape.
- Sorting and joins are not "extra credit" operators. They are where state management shows up fast.
- Once the executor contract is solid, the rest of the system can treat query execution like a black box.

## Sources
- `https://github.com/cmu-db/bustub`
- `/Users/otonashi/thinking/tmp/bustub/src/include/execution/plans/abstract_plan.h:33-64`
- `/Users/otonashi/thinking/tmp/bustub/src/include/execution/executors/abstract_executor.h:22-54`
- `/Users/otonashi/thinking/tmp/bustub/src/include/execution/execution_engine.h:29-85`
- `/Users/otonashi/thinking/tmp/bustub/src/execution/executor_factory.cpp:50-199`
- `/Users/otonashi/thinking/tmp/bustub/src/include/execution/execution_common.h:27-47`
- `/Users/otonashi/thinking/tmp/bustub/src/execution/execution_common.cpp:23-34`
- `/Users/otonashi/thinking/tmp/bustub/test/sql/p3.01-seqscan.slt`
- `/Users/otonashi/thinking/tmp/bustub/test/sql/p3.02-insert.slt`
- `/Users/otonashi/thinking/tmp/bustub/test/sql/p3.03-update.slt`
- `/Users/otonashi/thinking/tmp/bustub/test/sql/p3.04-delete.slt`
- `/Users/otonashi/thinking/tmp/bustub/test/sql/p3.05-index-scan-btree.slt`
- `/Users/otonashi/thinking/tmp/bustub/test/sql/p3.07-simple-agg.slt`
- `/Users/otonashi/thinking/tmp/bustub/test/sql/p3.10-simple-join.slt`
- `/Users/otonashi/thinking/tmp/bustub/test/sql/p3.14-hash-join.slt`
- `/Users/otonashi/thinking/tmp/bustub/test/sql/p3.16-sort-limit.slt`
- `/Users/otonashi/thinking/tmp/bustub/test/sql/p3.17-topn.slt`
- `/Users/otonashi/thinking/tmp/bustub/test/sql/p3.20-window-function.slt`
- `/Users/otonashi/thinking/tmp/bustub/test/sql/p3.22-composite-key-index-scan.slt`
- `/Users/otonashi/thinking/tmp/bustub/test/sql/p3.18-integration-1.slt`
- `/Users/otonashi/thinking/tmp/bustub/test/sql/p3.19-integration-2.slt`
