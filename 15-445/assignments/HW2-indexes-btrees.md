# 15-445 Homework 2: Storage Models, Extendible Hashing, and B+Trees

## Problem Statement
Homework 2 is the index-theory worksheet. It starts with storage-model I/O questions, then moves into extendible hashing, then finishes with B+Tree insertion, deletion, range scans, invariant checking, and latch discipline.

The point is not memorization. You are supposed to reason about pages: which layout wins, when a bucket splits, when a directory doubles, when a B+Tree redistributes, and when a tree is simply invalid.

## Key Techniques
- Compare decomposition storage model and N-ary storage model by page reads, not by vibes.
- Track global depth, local depth, and bucket fan-out in extendible hashing.
- Always redistribute before merging B+Tree nodes.
- Treat the leftmost leaf chain as the range-scan path.
- Use the B+Tree invariants as a checklist: key order, half-full, balance, and separator keys.
- Read the latch questions as a concurrency hint: real B+Tree code needs disciplined hand-over-hand locking.

## Implementation Walkthrough
1. Start with the page layouts. `BPlusTreePage` gives the shared header, `BPlusTreeLeafPage` stores keys, RIDs, tombstones, and the next leaf pointer, and `BPlusTreeInternalPage` stores separator keys plus child page IDs.
2. Follow the tree API. `BPlusTree` exposes `GetValue`, `Insert`, `Remove`, `Begin`, `Begin(key)`, and `End`, which is exactly the surface the homework is pointing at.
3. Use `IndexIterator` for range scans. The homework's "fetch all keys smaller than X" logic maps directly onto leaf-to-leaf traversal.
4. The index wrapper (`BPlusTreeIndex`) converts tuple keys into generic keys and forwards the actual work to the tree.
5. For extendible hashing, the page model is just as explicit: `ExtendibleHTableHeaderPage`, `ExtendibleHTableDirectoryPage`, and `ExtendibleHTableBucketPage` encode the global-depth/local-depth rules that the homework asks you to simulate.
6. The directory page's `VerifyIntegrity()` invariants are the code version of the worksheet's split/merge arithmetic.

## What You Learn
- Storage format changes query cost before any optimizer does.
- Extendible hashing is bookkeeping over bit prefixes.
- B+Trees are bookkeeping over order, occupancy, and sibling links.
- Ordered indexes buy you range scans; hash indexes buy you equality lookups.
- If the invariants are not written down, you will debug them by staring at broken pages later.

## Sources
- `https://15445.courses.cs.cmu.edu/fall2023/files/hw2-clean.pdf` (accessed 2026-04-02)
- `https://15445.courses.cs.cmu.edu/fall2023/files/hw2-clean.pdf` extracted with `summarize --extract --plain` (accessed 2026-04-02)
- `/Users/otonashi/thinking/tmp/bustub/src/include/storage/page/b_plus_tree_page.h:33-75`
- `/Users/otonashi/thinking/tmp/bustub/src/include/storage/page/b_plus_tree_leaf_page.h:31-120`
- `/Users/otonashi/thinking/tmp/bustub/src/include/storage/page/b_plus_tree_internal_page.h:27-98`
- `/Users/otonashi/thinking/tmp/bustub/src/include/storage/index/b_plus_tree.h:74-135`
- `/Users/otonashi/thinking/tmp/bustub/src/storage/index/b_plus_tree.cpp:52-135`
- `/Users/otonashi/thinking/tmp/bustub/src/include/storage/index/index_iterator.h:13-48`
- `/Users/otonashi/thinking/tmp/bustub/src/storage/index/index_iterator.cpp:26-41`
- `/Users/otonashi/thinking/tmp/bustub/src/storage/index/b_plus_tree_index.cpp:19-63`
- `/Users/otonashi/thinking/tmp/bustub/src/storage/index/extendible_hash_table_index.cpp:21-54`
- `/Users/otonashi/thinking/tmp/bustub/src/include/container/disk/hash/disk_extendible_hash_table.h:32-82`
- `/Users/otonashi/thinking/tmp/bustub/src/include/storage/page/extendible_htable_header_page.h:13-53`
- `/Users/otonashi/thinking/tmp/bustub/src/include/storage/page/extendible_htable_directory_page.h:13-131`
- `/Users/otonashi/thinking/tmp/bustub/src/include/storage/page/extendible_htable_bucket_page.h:13-85`
