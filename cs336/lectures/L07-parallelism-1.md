# L07: Parallelism 1

## Summary
- This is the first distributed-systems lecture: networking, collectives, and the basic split between data, model, and activation parallelism.
- Data parallelism is simple and effective, but it spends batch size and does not solve memory by itself.
- ZeRO turns data parallel into a memory story by sharding optimizer state, gradients, and then parameters.
- Pipeline parallelism is conceptually simple and operationally annoying.
- Tensor parallelism is the workhorse for intra-node model sharding when the interconnect is fast enough.

## Key Concepts
- **Collective communication:** all-reduce, reduce-scatter, all-gather, broadcast, send/recv.
- **NCCL:** the GPU collective library that underlies most of the real implementation.
- **Data parallel:** each rank gets different data, same full model.
- **ZeRO stages 1-3:** shard optimizer state, then gradients, then parameters.
- **Pipeline parallel:** cut the model by layers and ship activations between stages.
- **Tensor parallel:** split matrix multiplies across devices and synchronize activations.

## Non-Obvious Insights
- Batch size is a resource. The lecture treats it that way because many parallel strategies consume it.
- ZeRO stage 1 is the “free” win: same communication shape as naive DDP, less redundant memory.
- Pipeline parallel’s main problem is not the math. It is the bubble and the human cost of implementing the schedule.
- Tensor parallel looks expensive because it uses lots of bandwidth, but it is the cleanest fit for fast intra-node links.
- The lecture keeps repeating the same rule for a reason: the best parallelism strategy is topology-dependent, not ideology-dependent.
- FSDP and ZeRO are presented as practical answers to memory pressure, not as academic abstractions.

## Connections
- Connects back to MoE: once experts are sharded, expert parallelism is just another place where routing and topology meet.
- Connects to the GPU lectures because all these strategies still depend on memory traffic and kernel efficiency.
- This lecture is the bridge from “model design” to “cluster design.”

## Referenced Papers
- **ZeRO: Memory Optimizations Toward Training Trillion Parameter Models** - the sharded data-parallel baseline.
- **Megatron-LM: Training Multi-Billion Parameter Language Models Using Model Parallelism** - the canonical tensor/pipeline/data parallel reference.
- **Reducing Activation Recomputation in Large Transformer Models** - relevant to the activation-memory tradeoff discussed here.

## Timestamps
- **0:15** - the lecture’s multi-machine framing.
- **4:08** - intra-node versus inter-node communication.
- **6:02** - collective communication basics.
- **7:31** - all-reduce as the data-parallel primitive.
- **13:03** - the three parallelism families.
- **18:57** - why optimizer state sharding matters.
- **26:14** - ZeRO stage 1.
- **30:24** - ZeRO stage 3 and FSDP.
- **34:05** - overlapping communication and computation.
- **45:14** - why tensor parallel becomes necessary.
- **50:48** - pipeline parallel utilization bubbles.
- **56:03** - tensor parallel as matrix sharding.
- **1:14:57** - the simple rule of thumb for choosing parallelism.

Source: YouTube transcript extracted with `summarize --extract --timestamps --plain` from https://www.youtube.com/watch?v=l1RJcDjzK8M.
