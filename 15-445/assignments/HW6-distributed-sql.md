# HW6: Distributed SQL Query Plans

## Problem Statement
This homework is about making one logical SQL query run across multiple shards without hauling the whole database over the network. The prompt partitions tables, gives you a join shape, and asks you to choose shuffle keys and estimate network transfer. The real question is simple: how do you keep the data local long enough that the network stays cheap?

## Key Techniques
- Partition-by-key sharding.
- Shuffle on the join key so matching rows meet on the same fragment.
- Local hash joins inside each fragment.
- Partial aggregation before sending results upstream.
- Replicate small dimension tables when that is cheaper than moving them repeatedly.
- Track network transfer explicitly; distributed SQL is mostly data-movement accounting.

## Implementation Walkthrough
1. Start from the join graph. Use the equality predicate to choose the shuffle key that co-locates matching rows.
2. Push scans and joins down into fragments that can run locally after the shuffle. That keeps most work at the edge.
3. When the query has an aggregate, let each node compute a partial aggregate first. Send only the compact intermediate state to the central node for the final combine.
4. Replicate the small tables that are repeatedly joined everywhere instead of shuffling them over and over.
5. Use the plan shape to estimate transfer volume. A plan that moves full tables is expensive; a plan that localizes joins and aggregates is not.
6. The important mental shift is that SQL planning becomes network planning once the database is shared-nothing.

## What You Learn
- Join order matters even more when rows must cross machines.
- Partial aggregation is a network optimization, not just a CPU optimization.
- Replication is a cost tradeoff, not a moral choice.
- Distributed query planning is mostly about minimizing bytes, not maximizing abstraction.

## Sources
- Official prompt: [Fall 2023 HW5 PDF](https://15445.courses.cs.cmu.edu/fall2023/files/hw5-clean.pdf) (Question 3: Distributed Query Plan)
- The same PDF also covers WAL and 2PC, so the distributed plan sits next to the transaction-protocol questions in one assignment.
