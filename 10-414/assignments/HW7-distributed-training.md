# 10-414/714 Homework 7: Distributed Training

## Problem Statement
Training large models hits two hard limits:
- memory, because weights, optimizer state, and activations all want the same device memory
- throughput, because one GPU is not enough once the model or dataset gets large

The course material frames the solution as distributed training: split work across workers, keep gradients synchronized, and hide communication behind compute where possible.

## Key Techniques
- **Checkpointing.** Save only selected activations and recompute the missing ones during backward.
- **Data parallelism.** Replicate the model on every worker, split the minibatch, then use `allreduce` to sum gradients.
- **Parameter servers.** Use `push`/`pull` as an alternate synchronization abstraction when replica-based sync is not enough.
- **Communication overlap.** Start network transfer while later layers are still computing.
- **Model parallelism.** Partition the graph itself and insert `send`/`recv` boundaries between workers.
- **Tensor parallelism.** Shard tensors across devices and reconstruct or combine them with `allgather` or `allreduce`.
- **Sharded training paths.** ZeRO and FSDP are the next step once full replicas stop scaling.

## Implementation Walkthrough
1. Start from the single-device Needle training loop and keep the autograd core unchanged.
2. Add a communicator abstraction that exposes collectives like `allreduce()` and `allgather()`.
3. Build a data-parallel worker setup first: replicate the model, split the minibatch, run local forward/backward, then reduce gradients before the optimizer step.
4. Add overlap so gradient synchronization and later compute can proceed in parallel.
5. Extend the execution model to model parallelism by partitioning the graph and inserting explicit transfer edges.
6. Add tensor-parallel paths for large linear layers, where the tensor layout itself is split across devices.
7. Layer checkpointing on top of the distributed path so memory pressure does not dominate the scaling story.
8. Treat the synchronization primitive as part of the model runtime contract, not as an afterthought in the optimizer.

## What You Learn
- Distributed training is a memory problem as much as a scaling problem.
- Data parallelism is the baseline; tensor and model parallelism come later.
- Communication cost is the tax that decides whether a scaling strategy survives.
- The cleanest distributed systems expose a small set of collectives and keep everything else local.
- Once you can split a model across workers, the shape of the computation graph becomes a systems problem.

## Sources
- `https://dlsyscourse.org/slides/15-training-large-models.pdf`
- `https://dlsyscourse.org/lectures/`
- `https://api.github.com/orgs/dlsyscourse/repos?per_page=100`
