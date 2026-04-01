# L08: Parallelism 2

## Summary
- This lecture turns the parallelism taxonomy into code: `torch.distributed`, NCCL, and concrete sharding examples.
- The goal is not just speed. It is fitting a model into the hardware at all.
- Data parallel, tensor parallel, and pipeline parallel are all shown as executable patterns, not just slides.
- Communication overlap is the difference between theory and a system that actually runs well.
- Activation recomputation is the lever that buys back batch size and makes other forms of parallelism less painful.

## Key Concepts
- **NCCL:** GPU collective backend used for real distributed training.
- **PyTorch distributed:** the higher-level wrapper around collectives and process groups.
- **All-reduce / reduce-scatter / all-gather:** the core shuffle patterns for distributed training.
- **Data parallel:** shard the batch, keep the full model replicated.
- **Tensor parallel:** shard matrix multiplies and combine activations with collectives.
- **Pipeline parallel:** shard the model by layers and move activations stage to stage.

## Non-Obvious Insights
- The lecture’s code walkthrough makes the scheduling cost visible. The hard part is not only the communication primitive; it is when you issue it.
- Tensor parallel is happiest inside a node because it is bandwidth-hungry and latency-sensitive.
- Pipeline parallel is usually the fallback when the model does not fit, not the first thing you reach for.
- Batch size is again the hidden budget. Larger batches hide bubbles and make the whole stack easier to optimize.
- Activation recomputation is framed as a compute-for-memory trade, not as a pure optimization trick.
- The practical rule is clear: use the fastest links for tensor parallel, slower links for data parallel, and pipeline only when topology forces it.

## Connections
- This is the concrete continuation of Lecture 7. The conceptual taxonomy becomes actual PyTorch code.
- The lecture closes the loop on MoE by reusing the same distributed intuition for expert parallelism and routing.
- It also explains why big-model training stacks are so often a composition of 3D or 4D parallelism.

## Referenced Papers
- **Megatron-LM: Training Multi-Billion Parameter Language Models Using Model Parallelism** - the lecture’s main distributed-training reference.
- **ZeRO: Memory Optimizations Toward Training Trillion Parameter Models** - the memory-sharding baseline that informs FSDP.
- **FlashAttention-2** - relevant because activation recomputation and kernel efficiency both buy back usable batch size.

## Timestamps
- **4:21** - NCCL and PyTorch distributed in the implementation stack.
- **10:37** - NVLink bandwidth on H100s.
- **14:02** - NCCL topology discovery and collective dispatch.
- **19:09** - async communication and overlapping compute.
- **25:37** - benchmarking bandwidth rather than just latency.
- **33:05** - data parallel code walkthrough.
- **41:21** - tensor parallel code walkthrough.
- **46:27** - pipeline parallel code walkthrough.
- **57:40** - why the simple demos still miss real complexity.
- **1:02:30** - activation memory and recomputation.
- **1:12:25** - expert parallelism as the next extension.
- **1:17:22** - the rule of thumb for choosing between parallelism modes.

Source: YouTube transcript extracted with `summarize --extract --timestamps --plain` from https://www.youtube.com/watch?v=LHpr5ytssLo.
