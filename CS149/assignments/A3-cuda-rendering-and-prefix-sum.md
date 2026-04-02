# CS149 Assignment 3: CUDA Prefix Sum and Circle Rendering

## Problem Statement

Assignment 3 is the point where CS149 stops treating CUDA as "launch a kernel and hope" and starts treating it as a full programming model. The repo opens with a SAXPY warmup, escalates into a global-memory exclusive scan plus `find_repeats`, then spends most of the grade on a circle renderer that is mathematically complete but parallelized incorrectly.

The hard part is not writing CUDA syntax. The hard part is matching the machine's execution model. The scan portion asks for a tree-based prefix sum that launches only the useful threads at each level, not `N` threads with branchy no-op logic. The renderer portion asks for something stricter: preserve atomicity and preserve per-pixel circle order for transparent blending, while still running fast enough to compete with the staff reference. That is why the assignment keeps shifting between correctness and performance. A renderer that is fast but violates blending order is wrong, and a renderer that serializes everything is correct but misses the point.

## Key Techniques

- **Host/device data movement**: allocate GPU buffers, move inputs with `cudaMemcpy`, and time kernels correctly with `cudaDeviceSynchronize()` only where the measurement actually needs it.
- **Blelloch-style scan on padded arrays**: implement upsweep and downsweep over a power-of-two buffer, taking advantage of the starter code's next-power-of-two allocation.
- **Parallel compaction**: turn adjacent-equality tests into a flag array, scan the flags, then scatter matching indices into a compact output list for `find_repeats`.
- **Order-aware rendering decomposition**: stop thinking "one thread per circle" and instead organize work so every pixel's overlapping circles are applied in input order.
- **Spatial culling and tile-local worklists**: use the provided circle-box intersection helpers to avoid testing every circle against every pixel.
- **Shared-memory prefix scan as a building block**: use the supplied block-local exclusive scan to compact per-tile circle lists or other temporary metadata when that reduces global-memory traffic.

## Implementation Walkthrough

1. **Warm up with explicit CUDA plumbing.** In `saxpy/saxpy.cu`, `saxpyCuda` is intentionally incomplete. The expected path is straightforward: allocate device arrays, copy `X` and `Y` to the GPU, launch the SAXPY kernel, synchronize only around the kernel-only timer, copy results back, and free device memory. The warmup matters because the rest of the assignment assumes you can separate transfer cost from execution cost.
2. **Implement scan as a host-orchestrated tree.** The starter `exclusive_scan` in `scan/scan.cu` is a CPU-side function operating on device pointers. That means the host launches one CUDA kernel per upsweep level and one per downsweep level. The handout explicitly points out the key performance trap: do not launch `N` threads each round and mask most of them off. Launch exactly one thread per live tree node for that level.
3. **Build `find_repeats` out of scan and scatter.** The usual implementation pattern is: generate a device flag array where position `i` is `1` when `A[i] == A[i+1]`, exclusive-scan those flags, then scatter `i` into the output array at the scanned position for each flagged slot. The return value is the total number of matches, which you can recover from the last flag and last scanned value.
4. **Identify why the starter renderer is wrong before optimizing it.** `render/cudaRenderer.cu` launches `kernelRenderCircles` with one thread per circle, and `shadePixel(...)` reads, blends, and writes a pixel color directly in global memory. The README calls out the two failures: this update is not atomic, and overlapping circles can reach the same pixel in the wrong order. Because alpha blending is not commutative, either failure produces visible streaking and nondeterminism.
5. **Restructure rendering around pixels or tiles, not around circles alone.** A common full-credit strategy is tile-centric. Each block owns a tile of pixels, uses the `circleBoxTest.cu_inl` helpers to find which circles overlap that tile, compacts the hits into a tile-local list, then shades each pixel by walking that list in original circle order. This solves the ordering problem inside a tile and lets each thread accumulate pixel color in registers before a single global write. The provided shared-memory scan helper in `exclusiveScan.cu_inl` exists for exactly this kind of within-block compaction.
6. **Use the checkers as design feedback, not just as grading tools.** The scan directory has `./checker.py scan` and `./checker.py find_repeats`. The renderer has `./render --check` and `./checker.py`. The fastest way through the assignment is usually: make scan correct, make the renderer correct even if slow, measure where the renderer spends time, then iterate on culling, work assignment, and memory traffic.

## What You Learn

- CUDA performance starts with matching the work decomposition to the active work, not with micro-optimizing kernel syntax.
- Prefix sum is not just a standalone primitive. It is the gateway to compaction, filtering, and worklist construction.
- Graphics-style correctness constraints matter in parallel code. "Order only matters per pixel" is the crucial insight that creates parallelism without breaking blending.
- Register accumulation and tile-local reasoning often eliminate the need for expensive global synchronization.
- The assignment quietly teaches a broader lesson: correctness constraints define the legal optimization space.

## Sources

- Assignment handout: `https://github.com/stanford-cs149/asst3/blob/master/README.md`
- Scan starter: `https://github.com/stanford-cs149/asst3/blob/master/scan/scan.cu`
- SAXPY warmup: `https://github.com/stanford-cs149/asst3/blob/master/saxpy/saxpy.cu`
- CUDA renderer starter: `https://github.com/stanford-cs149/asst3/blob/master/render/cudaRenderer.cu`
- Shared-memory scan helper: `https://github.com/stanford-cs149/asst3/blob/master/render/exclusiveScan.cu_inl`
- Circle renderer reference implementation: `https://github.com/stanford-cs149/asst3/blob/master/render/refRenderer.cpp`
- GitHub repo: `https://github.com/stanford-cs149/asst3`
