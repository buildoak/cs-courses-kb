# CS149 Assignment 4: Programming a Machine Learning Accelerator

## Problem Statement

Assignment 4 moves the course from CUDA's thread/block model to AWS Trainium2's NeuronCore model. The repo used here is `asst4-trainium2`, which is the current public Assignment 4 starter repository even though older references still say `asst4`. The machine model is different enough that the assignment is really about memory choreography first and arithmetic second.

Part 1 teaches the Neuron Kernel Interface through vector add and matrix transpose. The lesson is that HBM, SBUF, and PSUM are software-managed, so the programmer decides what gets loaded, when it gets loaded, and how large each tile should be. Part 2 takes that model and asks for a real kernel: a fused convolution-plus-maxpool operator that must be correct on both small and large images and fast enough to hit staff latency thresholds. In practice, the assignment is about turning deep-learning math into a sequence of hardware-compatible tiled matrix operations without spilling unnecessary intermediates back to HBM.

## Key Techniques

- **Software-managed memory hierarchy**: reason explicitly about HBM capacity, SBUF locality, and PSUM accumulation instead of assuming caches will save bad access patterns.
- **Partition/free-dimension tiling**: respect the 128-element partition limit while using the free dimension to amortize DMA setup cost.
- **DMA-centric optimization**: treat `nisa.dma_copy` as a real cost center and reshape data so each transfer moves enough useful work.
- **Tensor-engine programming**: use `nisa.nc_transpose` for 128x128 transpose tiles and `nisa.nc_matmul` for tiled convolutions expressed as matrix multiplies.
- **Loop fusion**: keep convolution partial sums on chip, add bias, and optionally perform maxpool before writing final tiles to HBM.
- **Profiler-guided tuning**: use `neuron-profile` and the provided benchmark harnesses to decide whether the current bottleneck is DMA count, pipeline bubbles, or tensor-engine underutilization.

## Implementation Walkthrough

1. **Learn the memory model through vector add.** `part1/kernels.py` starts with `vector_add_naive`, which only works for vectors of length at most 128 because the vector length occupies the partition dimension. `vector_add_tiled` fixes correctness by chunking the vector into `ROW_CHUNK` slices, and `vector_add_stream` goes further by reshaping the input into `(128, FREE_DIM)` tiles so one DMA request moves many 128-row chunks at once.
2. **Use the free dimension to trade instruction count for locality.** The Part 1 questions are structured to make this visible. `ROW_CHUNK = 128` beats `ROW_CHUNK = 1` because it uses all 128 vector lanes. Increasing `FREE_DIM` in `vector_add_stream` reduces DMA setup overhead by moving larger tiles. Then the profiler section makes the next lesson explicit: larger tiles are not always faster if they create worse pipelining or more pressure on SBUF.
3. **Transpose is the first real tensor-engine kernel.** `matrix_transpose` in `part1/kernels.py` is a blank kernel that must operate on matrices whose dimensions are multiples of 128. The intended implementation is blockwise: iterate over 128x128 tiles, copy a tile from HBM into SBUF, run `nisa.nc_transpose` to get a PSUM tile, copy that tile into SBUF, and then DMA it back to the transposed location in HBM. That kernel forces you to internalize the difference between compute instructions and memory instructions on Trainium.
4. **Map convolution to repeated tiled matrix multiplies.** The Part 2 README does not want a direct spatial loop nest over every output pixel. It instead describes a reduction where each filter offset `(i, j)` produces a matrix multiply between a shifted view of the input and the corresponding slice of the weights. The practical implementation in `part2/conv2d.py` is therefore: choose an output tile shape that respects `pmax` and tensor-engine `moving/stationary` limits, reshape or transpose `X` and `W` into matmul-friendly layouts, accumulate partial sums in PSUM across input-channel tiles and filter offsets, and keep those partial sums on chip until the tile is complete.
5. **Fuse the post-processing before the HBM write.** The starter comments and README hint at the right split: compute convolution, add bias with a tensor operation, and when `pool_size == 2`, reduce each 2x2 region with a max reduction before storing. A good solution writes each output tile to HBM once, after convolution, bias, and optional pooling are all complete.
6. **Develop against the harness, then tune against the thresholds.** `part2/test_harness.py` runs correctness on small and large images, then benchmarks p99 latency against relaxed and optimized thresholds for `float32` and `float16`. That pushes a sane workflow: get the no-bias/no-pool case correct first, extend to large images, add bias, fuse maxpool, then profile the slow cases and restructure loops or tile shapes until the tensor engine stays busy.

## What You Learn

- Trainium programming is mostly about orchestrating data movement so the compute engines are never starved.
- Tile shape is not a boring constant. It is the main performance dial for DMA overhead, SBUF pressure, and engine pipelining.
- Tensor-engine kernels become manageable once you express them as tiled matmuls with explicit accumulation in PSUM.
- Fusion is valuable because it removes HBM round-trips, not because "fused" sounds advanced.
- Profilers matter more on accelerator code because the bottleneck can move between DMA engines, vector engines, and tensor engines as soon as you change one loop bound.

## Sources

- Assignment handout and Trainium2 repo: `https://github.com/stanford-cs149/asst4-trainium2/blob/master/README.md`
- Part 1 starter kernels: `https://github.com/stanford-cs149/asst4-trainium2/blob/master/part1/kernels.py`
- Part 2 fused kernel starter: `https://github.com/stanford-cs149/asst4-trainium2/blob/master/part2/conv2d.py`
- NumPy/PyTorch reference implementations: `https://github.com/stanford-cs149/asst4-trainium2/blob/master/part2/conv2d_numpy.py`
- Part 2 test harness: `https://github.com/stanford-cs149/asst4-trainium2/blob/master/part2/test_harness.py`
- GitHub repo used for this article: `https://github.com/stanford-cs149/asst4-trainium2`
