# P5: Distributed Query Execution in BusTub

## Problem Statement
This project is about running one logical query across multiple nodes without dragging every row back to a single machine. In the shared-nothing model, the query plan becomes a data-movement plan: push work to the data when you can, pull data to the query when you must, and keep network bytes small enough that the cluster is still worth having.

## Key Techniques
- Predicate pushdown, projection pushdown, and join-ordering still matter.
- Query plans get split into partition-specific fragments.
- Partitioned tables let joins stay local when the join key matches the shard key.
- Replicated small tables are cheaper to broadcast than to reshuffle repeatedly.
- Repartitioning on the join key is the fallback when the tables are not co-partitioned.
- Local hash joins and aggregation keep intermediate data compact.
- Intermediate results live in buffer-pool-backed pages, so the engine can survive results larger than RAM.

## Implementation Walkthrough
1. BusTub’s single-node execution path already looks like a fragment engine. `ExecutionEngine::Execute()` builds a root executor with `ExecutorFactory`, calls `Init()`, then repeatedly pulls batches with `Next()`. See `/Users/otonashi/thinking/pratchett-os/src/include/execution/execution_engine.h` and `/Users/otonashi/thinking/pratchett-os/src/include/execution/executor_factory.h`.
2. `ExecutorFactory::CreateExecutor()` recursively builds the operator tree. The factory wires scans, joins, aggregations, sorts, limits, and window functions into a plan-shaped DAG, which is exactly the shape a distributed planner later slices into fragments. See `/Users/otonashi/thinking/pratchett-os/src/execution/executor_factory.cpp`.
3. `ExecutorContext` carries the transaction, catalog, buffer pool manager, transaction manager, and lock manager into every executor. That is the state a distributed fragment would need whether it is local or remote. See `/Users/otonashi/thinking/pratchett-os/src/include/execution/executor_context.h`.
4. The distributed OLAP lecture says the engine can either push the query to the data or pull the data to the query node. It also shows the common join cases: replicated tables, co-partitioned tables, broadcast of a small table, and repartitioning when the join keys do not line up. See the Spring 2024 distributed OLAP slides.
5. The Fall 2023 distributed-query homework makes the same idea concrete with `Shuffle` and `HashJoin` fragments: choose the shuffle key from the join key, co-locate matching tuples, then join locally and union the fragment outputs. See the HW5 distributed query plan prompt.
6. Fault tolerance is a separate axis. The distributed OLAP lecture says most shared-nothing OLAP systems assume query failure if a node dies mid-query, and that intermediate snapshots are the obvious recovery hook. Inference: if a MiniRaft-style coordinator sits above the query layer, Raft’s replicated log and majority rule are the control-plane primitive, not the query operator itself.

## What You Learn
- Distributed SQL is mostly data-movement economics.
- Partitioning scheme decides whether a join is local, broadcast, or a network tax.
- Aggregation is a compression step as much as a computation step.
- Failure handling belongs in the system design, not in a postmortem.
- Consensus and query execution solve different problems. Raft keeps the cluster honest; the query engine keeps the tuples moving.

## Sources
- Spring 2024 lecture: [Distributed OLAP](https://15445.courses.cs.cmu.edu/spring2024/slides/24-distributedolap.pdf) (push vs pull, query plan fragments, replicated/co-partitioned/broadcast/repartitioned joins, query fault tolerance)
- Fall 2023 homework: [Homework #5 PDF](https://15445.courses.cs.cmu.edu/fall2023/files/hw5-clean.pdf) (Question 3: Distributed Query Plan)
- `/Users/otonashi/thinking/pratchett-os/src/include/execution/execution_engine.h` (single-node query execution loop)
- `/Users/otonashi/thinking/pratchett-os/src/include/execution/executor_factory.h` and `/Users/otonashi/thinking/pratchett-os/src/execution/executor_factory.cpp` (recursive plan-to-executor construction)
- `/Users/otonashi/thinking/pratchett-os/src/include/execution/executor_context.h` (catalog, BPM, txn manager, and lock manager passed into each executor)
- [Raft paper](https://www.scs.stanford.edu/~zyedidia/docs/papers/raft.pdf) (replicated log, leader election, safety, majority progress)
