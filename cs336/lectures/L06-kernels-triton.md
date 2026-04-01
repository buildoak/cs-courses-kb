# L06: Kernels, Triton

## Summary
- This lecture is about turning GPU performance ideas into code.
- Benchmark first, profile second. Otherwise the numbers lie.
- A hand-written CUDA kernel can still lose to PyTorch if the implementation is too naive.
- Triton sits in the middle: close enough to GPU details to matter, high-level enough to stay sane.
- torch.compile is the final warning shot: you should not hand-write kernels unless you have a real bottleneck.

## Key Concepts
- **Benchmarking:** measure end-to-end wall-clock time after warmup.
- **CUDA synchronize:** required when timing asynchronous GPU work.
- **Profiling:** tells you where the time went, not just how much time passed.
- **Kernel fusion:** a single kernel for a whole chain of pointwise ops.
- **Triton:** Python-facing DSL for GPU kernels, with block-level reasoning instead of thread-by-thread management.
- **PTX inspection:** the compiler output is low enough to show how coalescing and vectorization are actually happening.

## Non-Obvious Insights
- The lecture’s core discipline is operational, not theoretical: warm up, synchronize, then time.
- Profilers can expose a whole different bottleneck than the one you guessed from code inspection.
- A “manual” CUDA implementation is not automatically good. If it launches too many kernels, it loses.
- Triton’s value is not raw speed alone. It makes the fast path legible and debuggable.
- torch.compile is good enough that many bespoke kernels should never be written.
- FlashAttention-2 is a useful benchmark because it is hard enough to justify all the machinery but concrete enough to test against.

## Connections
- Builds on the previous GPU lecture’s memory hierarchy and warps.
- Feeds directly into later distributed systems work, where profiling matters just as much as algorithm choice.
- The same fusion logic appears again in attention, softmax, and MLP kernels across the course.

## Referenced Papers
- **FlashAttention-2** - the assignment target and the example of a high-performance attention kernel.
- **FlashAttention** - the original IO-aware attention formulation behind the optimization target.
- **Megatron-LM** - not the focus here, but a relevant reference point for serious GPU-scale training systems.

## Timestamps
- **0:55** - why benchmarking and profiling matter.
- **12:27** - CUDA synchronize and asynchronous GPU execution.
- **19:13** - profiler output on a no-op example.
- **21:10** - profiler output on a small vectorized add.
- **31:58** - fused kernels for core primitives like softmax and GELU.
- **44:58** - first CUDA kernel example.
- **57:37** - CUDA gelu benchmark result.
- **1:02:17** - Triton as the middle ground.
- **1:09:39** - PTX output and coalesced loads.
- **1:13:03** - torch.compile can beat hand-written code.
- **1:17:36** - softmax kernel in Triton.

Source: YouTube transcript extracted with `summarize --extract --timestamps --plain` from https://www.youtube.com/watch?v=E8Mju53VB00.
