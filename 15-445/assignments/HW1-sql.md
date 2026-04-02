# 15-445 Homework 1: SQL

## Problem Statement
Homework 1 is a query-writing sprint over the MusicBrainz dump. It asks for 10 SQL queries, with Q2-Q4 required in both SQLite and DuckDB so you can compare behavior and performance across two real engines.

The questions force you to work across the whole schema: `artist`, `artist_credit`, `artist_credit_name`, `release`, `release_info`, `medium`, `work`, and the lookup tables around them. The prompts mix joins, aggregation, string filtering, date extraction, ordering, and a final lateral-join-style query.

## Key Techniques
- Join the music schema by hand instead of hoping one table already has the answer.
- Use `GROUP BY`, `HAVING`, and tie-breaking `ORDER BY` clauses to turn English prompts into deterministic tuple sets.
- Reach for subqueries and CTEs when a question says "latest", "top", or "for each".
- Use string predicates like `LIKE`, suffix checks, and alias exclusion to separate the named artists from the noise.
- Use date parts and month/year grouping instead of treating release dates as flat strings.
- Create and reuse indexes on the MusicBrainz tables before you compare SQLite and DuckDB.

## Implementation Walkthrough
1. Start with the schema and indexes. The handout has you create indexes on artist, release, work, medium, and related tables before you solve the harder queries.
2. Treat Q1 as a formatting check, not a puzzle. If the output shape is wrong, the rest of the assignment is wasted time.
3. For Q2-Q4, build the joins first, then trim with `DISTINCT`, grouping, or max-per-group logic. Those questions are where the SQLite versus DuckDB comparison actually matters.
4. For Q5-Q8, use aggregation and filtering to collapse each artist or decade to one row, then sort with a stable tie-breaker.
5. For Q9-Q10, switch to heavier SQL: decade bucketing, fraction math, and lateral joins or correlated subqueries to pull the newest qualifying releases.
6. In BusTub terms, the homework mirrors the runtime path in `BusTubInstance::ExecuteSqlTxn`: parse with `Binder`, bind statements, plan with `Planner`, and hand the plan to the executor factory.

## What You Learn
- SQL is mostly about reducing ambiguity until one row remains per logical answer.
- The same question can need different formulations in different DBMSs.
- Indexes matter when the query shape matches the lookup pattern.
- Good SQL is as much about ordering and tie-breaking as it is about selection.
- The parser/binder/planner/executor pipeline is not abstract. It is the shape of the work.

## Sources
- `https://15445.courses.cs.cmu.edu/fall2023/homework1/` (accessed 2026-04-02)
- `https://15445.courses.cs.cmu.edu/fall2023/homework1/` extracted with `summarize --extract --plain` (accessed 2026-04-02)
- `/Users/otonashi/thinking/tmp/bustub/src/common/bustub_instance.cpp:250-373`
- `/Users/otonashi/thinking/tmp/bustub/src/binder/binder.cpp:60-73`
- `/Users/otonashi/thinking/tmp/bustub/src/binder/transformer.cpp:67-90`
- `/Users/otonashi/thinking/tmp/bustub/src/planner/planner.cpp:38-58`
- `/Users/otonashi/thinking/tmp/bustub/src/execution/executor_factory.cpp:111-141`
