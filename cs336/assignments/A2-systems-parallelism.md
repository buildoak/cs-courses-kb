# CS336 Assignment 2: Systems, Parallelism, and Optimizer State Sharding

## Problem Statement
Assignment 2 assumes the model from Assignment 1 already exists and asks a different question: how do you make it fast and scalable? The handout covers three layers of systems work: single-GPU benchmarking and profiling, custom Triton kernels for RMSNorm, and distributed training primitives including overlapping DDP communication and optimizer-state sharding.

This is not a theory-only assignment. The repository forces the student to prove real operational behavior: warm-up-aware benchmarking, profiler traces, autograd-compatible RMSNorm kernels, correct gradient synchronization across ranks, and parity between sharded and non-sharded optimization.

## Key Techniques
- **Measurement before optimization.** The first deliverables are benchmarking harnesses, PyTorch profiler runs, flame graphs, mixed-precision comparisons, and memory profiling.
- **Custom autograd plus Triton kernels.** RMSNorm is reimplemented twice: first as a handwritten `torch.autograd.Function` in PyTorch, then as fused Triton forward and backward kernels.
- **Communication/computation overlap.** DDP starts from the naive all-reduce-after-backward baseline, then improves by launching asynchronous gradient communication as soon as gradients are ready.
- **Gradient bucketing.** Instead of issuing one collective per parameter, the assignment groups gradients into buckets so it can reduce call overhead without giving up overlap.
- **Optimizer state sharding.** Rather than replicating all optimizer moments on every rank, each rank owns only a shard of parameters and optimizer state, updates that shard locally, then broadcasts updated parameters to keep the model synchronized.

## Implementation Walkthrough
1. **Build the measurement harness first.** The handout is explicit about warm-up steps and `torch.cuda.synchronize()` because naive timing of CUDA code lies. Before touching Triton or DDP, students need scripts that time forward and backward passes correctly and can export profiler tables and flame graphs.
2. **Write RMSNorm backward math before writing kernels.** The tests separate `rmsnorm_backward_g_pytorch`, `rmsnorm_backward_x_pytorch`, and the full autograd function. That forces a clean derivation of the Jacobian-vector products before any Triton optimization.
3. **Fuse RMSNorm in Triton once correctness is nailed down.** The Triton version reuses the same mathematical contract but changes the execution model: explicit pointer arithmetic, row-wise kernels, masking for block sizes, and a backward path that uses partial gradient buffers for the gain vector.
4. **Wrap modules in DDP containers instead of scattering logic through the training loop.** The public interface recommended by the handout and tests is a module wrapper with `forward(...)` and `finish_gradient_synchronization()`. That design keeps broadcast and gradient communication logic in one place.
5. **Handle ugly distributed edge cases, not just the happy path.** The provided test helpers include tied weights and non-trainable parameters. That is a signal: correct distributed training code has to respect parameter aliasing and ignore no-grad tensors without desynchronizing the model.
6. **Shard optimizer state by ownership, then synchronize parameters.** The sharded optimizer only updates the local shard of parameters with its wrapped optimizer. After the step, ranks broadcast updated parameters so the replicated model stays consistent while memory use drops.

## What You Learn
- GPU optimization is usually a memory-traffic and launch-overhead problem before it is a pure FLOPs problem.
- Good systems work starts with measurement. Without a profiler and clean benchmark harness, "optimization" is mostly storytelling.
- Distributed correctness is full of edge cases: tied weights, asynchronous collectives, bucket ordering, and when exactly gradients are considered ready.
- Optimizer-state sharding gives the intuition behind ZeRO stage 1: remove redundant optimizer memory first, then pay communication only where needed.

## Sources
- Handout: `https://github.com/stanford-cs336/spring2024-assignment2-systems/blob/main/cs336_spring2024_assignment2_systems.pdf` (Assignment 2, sections 2-4)
- Repo overview: `https://github.com/stanford-cs336/spring2024-assignment2-systems`
- Assignment 2 adapter contract: `/Users/otonashi/thinking/pratchett-os/tmp/research-008-a2/cs336-systems/tests/adapters.py`
- RMSNorm tests: `/Users/otonashi/thinking/pratchett-os/tmp/research-008-a2/cs336-systems/tests/test_rmsnorm.py`
- DDP tests: `/Users/otonashi/thinking/pratchett-os/tmp/research-008-a2/cs336-systems/tests/test_ddp_individual_parameters.py`
- Bucketed DDP tests: `/Users/otonashi/thinking/pratchett-os/tmp/research-008-a2/cs336-systems/tests/test_ddp.py`
- Sharded optimizer tests: `/Users/otonashi/thinking/pratchett-os/tmp/research-008-a2/cs336-systems/tests/test_sharded_optimizer.py`
- Distributed test helpers: `/Users/otonashi/thinking/pratchett-os/tmp/research-008-a2/cs336-systems/tests/common.py`
