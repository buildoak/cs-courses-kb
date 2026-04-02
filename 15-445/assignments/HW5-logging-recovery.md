# HW5: Logging, Recovery, and Checkpoints

## Problem Statement
This homework is the crash story. The first half asks you to apply write-ahead logging and ARIES to a log and decide what must be redone or undone after failure. The rest of the assignment checks whether you understand how durability interacts with two-phase commit and distributed query planning. In BusTub, that means the engine has to turn tuple changes into log records, flush them in order, and rebuild state after restart.

## Key Techniques
- Write-ahead logging.
- `LSN` and `prevLSN` chaining.
- ARIES-style redo and undo.
- Steal/no-force reasoning.
- Checkpoint barriers that flush the WAL before dirty pages.
- Commit coordination with 2PC when multiple nodes are involved.

## Implementation Walkthrough
1. `LogRecord` is the serialization boundary. It carries a record type plus the tuple images or page metadata needed to replay the action.
2. `LogManager` owns `LSN` allocation, the in-memory log buffers, and the flush thread. Its job is to make the log durable before the data pages it describes.
3. `LogRecovery` reads the log file back from disk, deserializes records, then runs `Redo()` and `Undo()` in the order required by recovery.
4. `CheckpointManager` blocks transactions long enough to force a consistent checkpoint: flush the WAL, flush dirty buffer pages, and only then let the system resume.
5. The disabled recovery tests show the intended contract: restart, verify the pre-recovery state is wrong, run redo and undo, then confirm the tuple set matches the committed outcome.
6. `BusTubInstance` wires the subsystem together at startup so the buffer pool, transaction manager, log manager, and checkpoint manager see the same catalog and disk state.

## What You Learn
- Recovery is controlled replay, not magic.
- The log is the source of truth when the buffer pool and disk disagree.
- Checkpoints are about bounding restart time.
- A durable DBMS is a protocol stack: write path, log path, flush path, replay path.

## Sources
- Official prompt: [Fall 2023 HW5 PDF](https://15445.courses.cs.cmu.edu/fall2023/files/hw5-clean.pdf) (Questions 1-2)
- [log_record.h](/Users/otonashi/thinking/tmp/bustub/src/include/recovery/log_record.h#L22)
- [log_record.h](/Users/otonashi/thinking/tmp/bustub/src/include/recovery/log_record.h#L38)
- [log_record.h](/Users/otonashi/thinking/tmp/bustub/src/include/recovery/log_record.h#L61)
- [log_manager.h](/Users/otonashi/thinking/tmp/bustub/src/include/recovery/log_manager.h#L25)
- [log_manager.h](/Users/otonashi/thinking/tmp/bustub/src/include/recovery/log_manager.h#L44)
- [log_manager.h](/Users/otonashi/thinking/tmp/bustub/src/include/recovery/log_manager.h#L57)
- [log_manager.cpp](/Users/otonashi/thinking/tmp/bustub/src/recovery/log_manager.cpp#L16)
- [log_manager.cpp](/Users/otonashi/thinking/tmp/bustub/src/recovery/log_manager.cpp#L25)
- [log_manager.cpp](/Users/otonashi/thinking/tmp/bustub/src/recovery/log_manager.cpp#L52)
- [log_recovery.h](/Users/otonashi/thinking/tmp/bustub/src/include/recovery/log_recovery.h#L25)
- [log_recovery.h](/Users/otonashi/thinking/tmp/bustub/src/include/recovery/log_recovery.h#L40)
- [checkpoint_manager.h](/Users/otonashi/thinking/tmp/bustub/src/include/recovery/checkpoint_manager.h#L21)
- [checkpoint_manager.cpp](/Users/otonashi/thinking/tmp/bustub/src/recovery/checkpoint_manager.cpp#L17)
- [bustub_instance.cpp](/Users/otonashi/thinking/tmp/bustub/src/common/bustub_instance.cpp#L63)
- [bustub_instance.cpp](/Users/otonashi/thinking/tmp/bustub/src/common/bustub_instance.cpp#L100)
- [recovery_test.cpp.disabled](/Users/otonashi/thinking/tmp/bustub/test/recovery/recovery_test.cpp.disabled#L56)
- [recovery_test.cpp.disabled](/Users/otonashi/thinking/tmp/bustub/test/recovery/recovery_test.cpp.disabled#L145)
- [recovery_test.cpp.disabled](/Users/otonashi/thinking/tmp/bustub/test/recovery/recovery_test.cpp.disabled#L224)
