# L05: GPUs

## Summary
- The lecture demystifies GPU execution: SMs, warps, memory hierarchy, and tensor cores.
- Performance is mostly a memory story, not a FLOPs story.
- The roofline model is the mental model for deciding whether you are compute-bound or memory-bound.
- Coalescing, tiling, and fusion are the actual levers that move GPU performance.
- FlashAttention is used as the end-to-end payoff: the same principles become a real kernel.

## Key Concepts
- **SMs:** streaming multiprocessors, the atomic scheduling unit for GPU work.
- **Warps:** 32-thread execution groups. Branching inside a warp is expensive because the hardware executes the same instruction across lanes.
- **Memory hierarchy:** registers and shared memory are fast; global memory is slow; L2 sits in between.
- **Tensor cores:** specialized matrix-multiply hardware that explains the big gap between matmul and non-matmul throughput.
- **Roofline model:** throughput is limited either by arithmetic intensity or by memory bandwidth.
- **Kernel fusion:** merge multiple small operations into one kernel to avoid repeated global-memory traffic.

## Non-Obvious Insights
- The lecture makes a strong claim: modern GPU programming is mostly about reducing memory traffic, not writing clever math.
- Occupancy is not abstract. Padding a vocabulary or matrix dimension to fit hardware granularity can produce real speedups.
- Tensor cores are why matmul looks so different from everything else. The hardware is asymmetric on purpose.
- Memory coalescing is a hardware-level bargain: make contiguous accesses and the machine gives you bandwidth you do not otherwise get.
- FlashAttention is presented as the synthesis of the lecture: tiled compute, fused operations, and less memory movement.

## Connections
- Direct prelude to the Triton lecture. The same execution model becomes code you can write.
- The roofline model returns in later systems work: every distributed strategy is still trading compute, bandwidth, and memory.
- FlashAttention ties the hardware lecture back to the model lecture. Architecture wins when it respects device constraints.

## Referenced Papers
- **FlashAttention** - the attention kernel that makes the memory-hierarchy argument concrete.
- **FlashAttention-2** (Dao, 2023) - better work partitioning and occupancy for the same class of attention kernels.

## Timestamps
- **0:27** - GPUs as the machine that makes language models go.
- **8:58** - Triton thinks at the SM level.
- **11:23** - memory hierarchy: L1/shared memory versus L2 versus global memory.
- **14:12** - blocks, warps, and threads.
- **21:22** - tensor cores versus non-matmul FLOPs.
- **26:31** - roofline model.
- **33:27** - operator fusion as a performance lever.
- **44:22** - memory coalescing.
- **49:35** - tiling the matrix multiply.
- **1:03:47** - the four core tricks: coalescing, fusion, shared memory, tiling.
- **1:05:20** - FlashAttention as the combined result.

Source: YouTube transcript extracted with `summarize --extract --timestamps --plain` from https://www.youtube.com/watch?v=6OBtO9niT00.
