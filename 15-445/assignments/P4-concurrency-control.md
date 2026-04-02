# P4: Concurrency Control in BusTub

## Problem Statement
This project is about making a database stop pretending it is single-threaded. BusTub adds a lock manager, then threads that lock manager through table access and executor code so concurrent transactions can block, wait, upgrade, or abort instead of corrupting each other. The spec is explicit: table locks, tuple locks, five lock modes, isolation-level checks, lock upgrades, and deadlock detection all have to work together.

## Key Techniques
- Hierarchical locking with `IS`, `IX`, `SIX`, `S`, and `X`.
- FIFO request queues per resource, with a condition variable to wake blocked transactions.
- Lock upgrades with a single upgrader per resource.
- Waits-for graph cycle detection, with the youngest transaction in the cycle chosen as the victim.
- Transaction bookkeeping for table locks, row locks, write sets, and page sets.
- Abort cleanup that releases locks through the transaction manager instead of ad hoc unlocking.

## Implementation Walkthrough
1. The 2023spring `Transaction` object is the lock manager’s memory. It stores transaction state, isolation level, write set, page set, and the per-table/per-row lock sets the rest of the system needs to release later. See `git show v20230514-2023spring:src/include/concurrency/transaction.h`.
2. `TransactionManager::Begin()` allocates a transaction, assigns the next id, and puts it into the global transaction map. `Commit()` and `Abort()` both release locks through `ReleaseLocks()` before flipping the state. See `git show v20230514-2023spring:src/include/concurrency/transaction_manager.h` and `git show v20230514-2023spring:src/concurrency/transaction_manager.cpp`.
3. `LockManager` is the policy engine. The header spells out the blocking behavior, lock compatibility, isolation-level restrictions, row-versus-table constraints, upgrade rules, and unlock rules. The queue object keeps the wait list, the upgrader slot, and the condition variable together. See `git show v20230514-2023spring:src/include/concurrency/lock_manager.h`.
4. Deadlock handling is separate from basic locking. The manager maintains a waits-for graph, detects cycles in a background thread, and aborts the youngest transaction in the cycle. The stub implementation in `lock_manager.cpp` shows the required graph API and cycle loop. See `git show v20230514-2023spring:src/concurrency/lock_manager.cpp`.
5. `TableHeap::InsertTuple()` is where lock manager behavior becomes visible to storage. It inserts the tuple, then acquires an exclusive row lock on the new `RID` through `LockManager::LockRow()` before returning. That is why the insert path can participate in transaction control without special casing elsewhere. See `git show v20230514-2023spring:src/storage/table/table_heap.cpp`.
6. The executor side receives the same transaction control plane through `ExecutorContext`, which carries the transaction, catalog, buffer pool manager, transaction manager, and lock manager. That context is what lets scan/insert/delete code participate in 2PL without global state. See `git show v20230514-2023spring:src/include/execution/executor_context.h`.

## What You Learn
- Concurrency control is mostly state management, not mutex theater.
- Deadlock detection is easier to reason about when it is isolated from lock grant logic.
- Lock upgrades are a real design problem, not a footnote.
- Transaction metadata is the control plane for the whole system.
- The executor layer only stays sane when it can ask the lock manager the same question every time: may I touch this tuple now?

## Sources
- Official spec: [Project #4 - Concurrency Control](https://15445.courses.cs.cmu.edu/spring2023/project4/) (overview, Task #1 Lock Manager, Task #2 Deadlock Detection, Task #3 Concurrent Query Execution)
- `git show v20230514-2023spring:src/include/concurrency/transaction.h` (transaction state, isolation level, lock sets, abort reasons)
- `git show v20230514-2023spring:src/include/concurrency/transaction_manager.h` (transaction map, begin/commit/abort, lock release path)
- `git show v20230514-2023spring:src/concurrency/transaction_manager.cpp` (commit/abort behavior)
- `git show v20230514-2023spring:src/include/concurrency/lock_manager.h` (lock modes, queues, upgrade rules, unlock rules)
- `git show v20230514-2023spring:src/concurrency/lock_manager.cpp` (deadlock graph API and detection loop)
- `git show v20230514-2023spring:src/storage/table/table_heap.cpp` (insert path grabs row locks atomically)
- `git show v20230514-2023spring:src/include/execution/executor_context.h` (transaction/lock manager propagation into executors)
