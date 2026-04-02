# 10-414/714 Homework 6: GPU/CUDA Backend for Needle

## Problem Statement
Needle has to stop being a Python-only array wrapper and become a real backend.

This homework is about making `nd.NDArray(..., device=nd.cuda())` work by splitting the system into three layers:
- Python-side view logic in `python/needle/backend_ndarray/ndarray.py`
- CPU reference kernels in `src/ndarray_backend_cpu.cc`
- CUDA kernels in `src/ndarray_backend_cuda.cu`

The core problem is not "add a GPU flag." The problem is to keep shape/stride semantics in Python, keep raw memory ops in native code, and make the backend behave like a compact array library instead of a pile of copies.

## Key Techniques
- **Zero-copy views.** `reshape`, `permute`, `broadcast_to`, and `__getitem__` should reuse the same storage and only change shape/stride metadata.
- **Compact before heavy ops.** Operations that need contiguous layout should call `compact()` first instead of trying to reason about arbitrary strides inside every kernel.
- **Host/kernel split.** Each CUDA operation has a host wrapper and a `__global__` kernel. The wrapper handles allocation and launch; the kernel does the work.
- **Shape metadata via `CudaVec`.** CUDA kernels cannot use `std::vector`, so the homework passes shape and stride metadata through the provided `CudaVec` / `VecToCuda()` path.
- **CPU as reference.** The CPU backend in `ndarray_backend_cpu.cc` is the correctness baseline before the CUDA version is tuned.
- **Tiling for matmul.** The final speed win comes from cooperative fetch, shared memory, and register tiling, not from a naive one-thread-per-output implementation.

## Implementation Walkthrough
1. Read the backend contract in `python/needle/backend_ndarray/ndarray.py` and the stub device backend in `python/needle/backend_ndarray/ndarray_backend_numpy.py`.
2. Implement the pure Python layout operations first: `reshape()`, `permute()`, `broadcast_to()`, and `__getitem__()`.
3. Fill in the CPU primitives in `src/ndarray_backend_cpu.cc`.
4. Start with `Compact()`, `EwiseSetitem()`, and `ScalarSetitem()`, because the compact/setitem code path teaches the indexing pattern that the later ops reuse.
5. Add the elementwise and scalar ops, then reductions, then `Matmul()`.
6. Mirror the same functionality in `src/ndarray_backend_cuda.cu`, using the provided CUDA kernel patterns and the `CudaVec` metadata bridge.
7. Re-enable the relevant Pybind11 exports, rebuild with `make`, and run the hw3 tests.
8. Verify the backend selection path from Python by creating arrays on `nd.cuda()` and checking that the CUDA path actually executes.

## What You Learn
- Layout is the real abstraction boundary in array libraries.
- Copy avoidance matters more than clever syntax.
- A GPU backend is mostly about memory traffic, launch structure, and contiguous access.
- The Python/C++/CUDA split only works if the metadata contract is clean.
- Once the backend is correct, the rest of Needle can stay small.

## Sources
- `https://raw.githubusercontent.com/dlsyscourse/hw3/main/hw3.ipynb`
- `https://raw.githubusercontent.com/dlsyscourse/lecture13/main/13_hardware_acceleration_architecture_overview.ipynb`
- `https://raw.githubusercontent.com/dlsyscourse/lecture13/main/14_hardware_acceleration_architecture_overview.ipynb`
- `https://dlsyscourse.org/slides/12-gpu-acceleration.pdf`
- `https://github.com/dlsyscourse/hw3`
