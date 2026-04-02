# 15-445 Project 2: B+Tree Index

## Problem Statement
Project 2 turns BusTub into an ordered index. The tree must support unique keys, insert and remove, dynamic split and merge behavior, and range scans through an iterator. The real work is not "search a tree." It is maintaining a page-organized structure where leaves store data, internal pages route the search, and sibling links make scans possible. [/Users/otonashi/thinking/tmp/bustub/src/include/storage/index/b_plus_tree.h:13-21] [/Users/otonashi/thinking/tmp/bustub/src/include/storage/index/b_plus_tree.h:74-105]

## Key Techniques
- Model the tree as pages, not nodes in the abstract. `BPlusTreePage` gives the shared header, leaf pages carry keys, RIDs, tombstones, and a next-leaf pointer, and internal pages carry separator keys plus child page ids. [/Users/otonashi/thinking/tmp/bustub/src/include/storage/page/b_plus_tree_page.h:33-75] [/Users/otonashi/thinking/tmp/bustub/src/include/storage/page/b_plus_tree_leaf_page.h:31-120] [/Users/otonashi/thinking/tmp/bustub/src/include/storage/page/b_plus_tree_internal_page.h:27-97]
- Keep insertion and deletion state in a `Context`. The tree uses it to remember the header-page guard, the root page id, and the page guards it is actively modifying. [/Users/otonashi/thinking/tmp/bustub/src/include/storage/index/b_plus_tree.h:48-69]
- Treat `IndexIterator` as the range-scan surface. `Begin()`, `Begin(key)`, and `End()` are the contract, and the iterator itself only needs to know how to walk the leaf chain. [/Users/otonashi/thinking/tmp/bustub/src/include/storage/index/index_iterator.h:13-48] [/Users/otonashi/thinking/tmp/bustub/src/storage/index/index_iterator.cpp:22-41]
- Use the index wrapper as a translation layer. `BPlusTreeIndex` converts SQL tuple keys into `GenericKey` values and forwards insert, delete, and scan into the tree. [/Users/otonashi/thinking/tmp/bustub/src/storage/index/b_plus_tree_index.cpp:19-54]
- Validate shape after every structural change. The tests stress insert, point lookup, iteration, deletion, root collapse, and mixed insert/delete sequences. [/Users/otonashi/thinking/tmp/bustub/test/storage/b_plus_tree_insert_test.cpp:27-213] [/Users/otonashi/thinking/tmp/bustub/test/storage/b_plus_tree_delete_test.cpp:27-195]

## Implementation Walkthrough
1. Start with the layout. `BPlusTreeLeafPage` stores a fixed tombstone buffer, ordered keys, RIDs, and the next-page link; `BPlusTreeInternalPage` stores separator keys and child page ids, with the first key intentionally invalid. [/Users/otonashi/thinking/tmp/bustub/src/include/storage/page/b_plus_tree_leaf_page.h:31-120] [/Users/otonashi/thinking/tmp/bustub/src/include/storage/page/b_plus_tree_internal_page.h:27-97]
2. Wire the tree constructor to the header page. `BPlusTree` opens the header page, stores `INVALID_PAGE_ID` as the initial root, and keeps the buffer pool wrapper plus comparator around for later operations. [/Users/otonashi/thinking/tmp/bustub/src/storage/index/b_plus_tree.cpp:19-31] [/Users/otonashi/thinking/tmp/bustub/src/include/storage/index/b_plus_tree.h:121-137]
3. Implement `GetValue()` as a root-to-leaf descent plus in-leaf search. The code path is split out in `b_plus_tree.cpp`, and the TODO markers show exactly where point lookup, insertion, deletion, and root access live. [/Users/otonashi/thinking/tmp/bustub/src/storage/index/b_plus_tree.cpp:37-135]
4. Implement `Insert()` and `Remove()` with split/redistribute/merge logic. The comments spell out the contract: unique keys on insert, leaf deletion first, then fix-up if the page underflows, and shrink the root when the tree becomes empty. [/Users/otonashi/thinking/tmp/bustub/src/storage/index/b_plus_tree.cpp:62-97]
5. Finish the iterator path last. Once `Begin()` and `Begin(key)` can find the right leaf, `operator++` just advances across the leaf chain until `End()` appears. [/Users/otonashi/thinking/tmp/bustub/src/include/storage/index/index_iterator.h:29-48] [/Users/otonashi/thinking/tmp/bustub/src/storage/index/index_iterator.cpp:26-41]
6. Use the tests as the mental checklist. Basic insert expects the tree to start as a single leaf, iterator tests expect sorted key order, and delete tests expect the root to collapse back to `INVALID_PAGE_ID` when the final key is removed. [/Users/otonashi/thinking/tmp/bustub/test/storage/b_plus_tree_insert_test.cpp:27-213] [/Users/otonashi/thinking/tmp/bustub/test/storage/b_plus_tree_delete_test.cpp:27-195]

## What You Learn
- A B+Tree is a page protocol, not a data-structure textbook diagram.
- The hard part is structural maintenance: split points, separator keys, sibling pointers, and root promotion/demotion.
- Ordered indexes are valuable because they give you both point lookup and range scan from the same storage shape.
- Good index code is mostly invariant preservation with a small amount of search logic on top.

## Sources
- `https://github.com/cmu-db/bustub`
- `/Users/otonashi/thinking/tmp/bustub/src/include/storage/index/b_plus_tree.h:13-21`
- `/Users/otonashi/thinking/tmp/bustub/src/include/storage/index/b_plus_tree.h:48-137`
- `/Users/otonashi/thinking/tmp/bustub/src/include/storage/page/b_plus_tree_page.h:33-75`
- `/Users/otonashi/thinking/tmp/bustub/src/include/storage/page/b_plus_tree_leaf_page.h:31-120`
- `/Users/otonashi/thinking/tmp/bustub/src/include/storage/page/b_plus_tree_internal_page.h:27-97`
- `/Users/otonashi/thinking/tmp/bustub/src/storage/index/b_plus_tree.cpp:19-135`
- `/Users/otonashi/thinking/tmp/bustub/src/include/storage/index/index_iterator.h:13-48`
- `/Users/otonashi/thinking/tmp/bustub/src/storage/index/index_iterator.cpp:22-41`
- `/Users/otonashi/thinking/tmp/bustub/src/storage/index/b_plus_tree_index.cpp:19-54`
- `/Users/otonashi/thinking/tmp/bustub/test/storage/b_plus_tree_insert_test.cpp:27-213`
- `/Users/otonashi/thinking/tmp/bustub/test/storage/b_plus_tree_delete_test.cpp:27-195`

