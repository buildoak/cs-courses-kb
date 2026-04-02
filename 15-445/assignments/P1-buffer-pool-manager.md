# 15-445 Project 1: Buffer Pool Manager

## Problem Statement
BusTub's first project is the cache layer between disk and memory. The buffer pool owns frame headers, a page table, a free-frame list, and an ARC replacer, then exposes page data only through read and write guards. The design is intentionally strict: pages live in per-frame vectors so address sanitizer can catch overflows, and pin counts decide what can stay resident. [README.md:7-9] [/Users/otonashi/thinking/tmp/bustub/src/include/buffer/buffer_pool_manager.h:35-57] [/Users/otonashi/thinking/tmp/bustub/src/include/buffer/buffer_pool_manager.h:100-173]

## Key Techniques
- Use a monotonically increasing atomic page id so page allocation stays thread-safe. [/Users/otonashi/thinking/tmp/bustub/src/buffer/buffer_pool_manager.cpp:108-120]
- Keep the page table, free-frame list, and replacer in sync so the manager can find a live frame or evict one without guessing. [/Users/otonashi/thinking/tmp/bustub/src/include/buffer/buffer_pool_manager.h:129-173]
- Treat `ReadPageGuard` and `WritePageGuard` as the only legal way to touch page bytes. They enforce shared vs exclusive access and handle latch lifetime through RAII. [/Users/otonashi/thinking/tmp/bustub/src/include/storage/page/page_guard.h:27-35] [/Users/otonashi/thinking/tmp/bustub/src/include/storage/page/page_guard.h:127-136]
- Make pin count transitions explicit. The tests check that `Drop()` unpins, move construction/assignment preserve validity rules, and flushing survives guard destruction. [/Users/otonashi/thinking/tmp/bustub/test/buffer/buffer_pool_manager_test.cpp:63-237] [/Users/otonashi/thinking/tmp/bustub/test/storage/page_guard_test.cpp:25-108]
- Separate safe and unsafe flush paths. The manager offers page-level and full-pool flushes, with and without page latches, because eviction and writeback are not the same operation. [/Users/otonashi/thinking/tmp/bustub/src/buffer/buffer_pool_manager.cpp:264-332]

## Implementation Walkthrough
1. Start from the storage model. `BufferPoolManager` sits on top of `DiskScheduler` and `ArcReplacer`, while `FrameHeader` holds the raw bytes, a shared mutex, a dirty bit, and an atomic pin counter. [/Users/otonashi/thinking/tmp/bustub/src/include/buffer/buffer_pool_manager.h:35-89] [/Users/otonashi/thinking/tmp/bustub/src/include/buffer/buffer_pool_manager.h:100-173]
2. Implement page allocation and deletion first. `NewPage()` hands out a fresh id, and `DeletePage()` has to check whether the page is still pinned, remove it from the page table if needed, and deallocate the disk slot. [/Users/otonashi/thinking/tmp/bustub/src/buffer/buffer_pool_manager.cpp:108-141]
3. Build the guarded access path next. `CheckedWritePage()` and `CheckedReadPage()` are the hard part: they handle the "already resident", "free frame available", and "need eviction" cases, then return RAII guards that keep the frame pinned until drop. [/Users/otonashi/thinking/tmp/bustub/src/buffer/buffer_pool_manager.cpp:143-212]
4. Finish with flush and accounting. The flush APIs push dirty pages back to disk, and `GetPinCount()` is the test hook that proves the manager is releasing pages when the guards go away. [/Users/otonashi/thinking/tmp/bustub/src/buffer/buffer_pool_manager.cpp:214-360]
5. The tests show the contract clearly: write, read, drop, refill the pool, force eviction, then prove the old contents still come back from disk after the frame is reused. [/Users/otonashi/thinking/tmp/bustub/test/buffer/buffer_pool_manager_test.cpp:32-237] [/Users/otonashi/thinking/tmp/bustub/test/storage/page_guard_test.cpp:25-108]

## What You Learn
- Buffer management is not a bookkeeping exercise. It is concurrency, eviction, and persistence glued together by very small invariants.
- RAII is the right abstraction for latch release and pin release. Manual cleanup here is how deadlocks and leaked pins happen.
- The page table is a cache index, not a storage layout. If it drifts from the frame state, everything upstream lies.
- Buffer pool bugs are contagious. A bad pin count or stale dirty bit shows up later as "random" failures in indexing and execution.

## Sources
- `https://github.com/cmu-db/bustub`
- `/Users/otonashi/thinking/tmp/bustub/README.md:7-9`
- `/Users/otonashi/thinking/tmp/bustub/src/include/buffer/buffer_pool_manager.h:35-57`
- `/Users/otonashi/thinking/tmp/bustub/src/include/buffer/buffer_pool_manager.h:100-173`
- `/Users/otonashi/thinking/tmp/bustub/src/include/storage/page/page_guard.h:27-35`
- `/Users/otonashi/thinking/tmp/bustub/src/include/storage/page/page_guard.h:127-136`
- `/Users/otonashi/thinking/tmp/bustub/src/buffer/buffer_pool_manager.cpp:71-360`
- `/Users/otonashi/thinking/tmp/bustub/test/buffer/buffer_pool_manager_test.cpp:32-237`
- `/Users/otonashi/thinking/tmp/bustub/test/storage/page_guard_test.cpp:25-108`

