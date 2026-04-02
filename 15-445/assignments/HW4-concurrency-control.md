# HW4: Concurrency Control in BusTub

## Problem Statement
This homework is about making concurrent database work behave like a serial schedule without pretending the world is single-threaded. The prompt walks through query execution, serializability, strong strict 2PL, hierarchical locking, and optimistic concurrency control. In BusTub terms, that turns into a transaction layer that has to track state, timestamps, undo history, and lock ownership without lying about isolation.

## Key Techniques
- Conflict graphs and serializability checks.
- Strong strict 2PL versus regular 2PL.
- Multi-granularity locking with `S`, `X`, `IS`, `IX`, and `SIX`.
- OCC validation against read and write sets.
- Transaction state transitions: running, tainted, committed, aborted.
- Watermark tracking for low-watermark cleanup.

## Implementation Walkthrough
1. `Transaction` stores the per-transaction facts the rest of the system depends on: isolation level, thread id, state, read and commit timestamps, undo logs, write sets, and scan predicates.
2. `TransactionManager::Begin()` creates the transaction, inserts it into the transaction map, and hooks it into the running-txn watermark so the engine can reason about visibility and garbage collection.
3. `Commit()` serializes commits through `commit_mutex_`, verifies serializable transactions when needed, stamps the commit timestamp, marks the transaction committed, and removes it from the watermark.
4. `Abort()` flips running or tainted transactions into `ABORTED` and removes them from the active-read set.
5. `LockManager` is where the homework's lock rules become code: FIFO queues, lock-mode compatibility, upgrade rules, row-versus-table constraints, and deadlock detection.
6. `Watermark` tracks the minimum active read timestamp. That is the knob that lets MVCC-style cleanup know when old versions are safe to discard.

## What You Learn
- Serializability is a contract, not a slogan.
- The hard parts are edge cases: upgrades, shrinking phases, and abort cleanup.
- Locking and versioning solve different problems; a real DBMS usually needs both.
- Transaction metadata is not bookkeeping noise. It is the control plane for the whole engine.

## Sources
- Official prompt: [Fall 2023 HW4 PDF](https://15445.courses.cs.cmu.edu/fall2023/files/hw4-clean.pdf) (Questions 2-4)
- [transaction.h](/Users/otonashi/thinking/tmp/bustub/src/include/concurrency/transaction.h#L44)
- [transaction.h](/Users/otonashi/thinking/tmp/bustub/src/include/concurrency/transaction.h#L73)
- [transaction.h](/Users/otonashi/thinking/tmp/bustub/src/include/concurrency/transaction.h#L175)
- [transaction_manager.h](/Users/otonashi/thinking/tmp/bustub/src/include/concurrency/transaction_manager.h#L42)
- [transaction_manager.h](/Users/otonashi/thinking/tmp/bustub/src/include/concurrency/transaction_manager.h#L57)
- [transaction_manager.h](/Users/otonashi/thinking/tmp/bustub/src/include/concurrency/transaction_manager.h#L85)
- [transaction_manager.cpp](/Users/otonashi/thinking/tmp/bustub/src/concurrency/transaction_manager.cpp#L43)
- [transaction_manager.cpp](/Users/otonashi/thinking/tmp/bustub/src/concurrency/transaction_manager.cpp#L65)
- [transaction_manager.cpp](/Users/otonashi/thinking/tmp/bustub/src/concurrency/transaction_manager.cpp#L99)
- [watermark.h](/Users/otonashi/thinking/tmp/bustub/src/include/concurrency/watermark.h#L26)
- [watermark.cpp](/Users/otonashi/thinking/tmp/bustub/src/concurrency/watermark.cpp#L19)
- [lock_manager.h](/Users/otonashi/thinking/tmp/bustub/src/include/concurrency/lock_manager.h#L37)
- [lock_manager.h](/Users/otonashi/thinking/tmp/bustub/src/include/concurrency/lock_manager.h#L103)
- [lock_manager.h](/Users/otonashi/thinking/tmp/bustub/src/include/concurrency/lock_manager.h#L190)
