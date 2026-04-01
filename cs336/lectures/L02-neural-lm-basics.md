# L02 Neural LM Basics

Lecture 2 is really a resource-accounting lecture disguised as a model-building lecture. The point is not “here is a transformer.” The point is “here is how to reason about training at all.”

## Summary
- The lecture frames training as napkin-math first: FLOPs, memory, and hardware throughput before model glamour. Source: transcript `[0:48]` to `[3:25]` in `/Users/otonashi/thinking/pratchett-os/tmp/research-001-l02-transcript.txt`.
- Tensors are the atom of the stack. Parameters, gradients, optimizer state, data, and activations all reduce to tensors with shape and dtype. Source: `[5:20]` to `[6:00]`.
- Precision choice is an engineering tradeoff. float32 is the default baseline, bf16 is usually the practical training format, and some components still need float32. Source: `[6:06]` to `[14:05]`.
- PyTorch mechanics matter because storage, views, contiguity, and device placement affect both correctness and speed. Source: `[17:23]` to `[23:21]`.
- Training cost is dominated by forward, backward, and optimizer state. The lecture derives the familiar `6 x parameters x tokens` rule of thumb and the memory footprint of AdamW-like training. Source: `[52:41]` to `[59:25]`, `[1:10:35]` to `[1:12:34]`.

## Key Concepts
- **Tensor as storage primitive.** A tensor is not just numbers. It is data plus metadata plus layout, and that layout determines whether operations are cheap or expensive. Source: `[17:36]` to `[19:17]`.
- **dtype is policy.** float32 offers range and stability; bf16 gives most of the memory savings with less numerical pain; FP16 can underflow more easily. Source: `[6:15]` to `[12:25]`.
- **Memory is not just parameters.** The full bill includes parameters, gradients, optimizer states, and activations. Source: `[1:10:35]` to `[1:11:58]`.
- **Device placement is explicit.** Tensors live on CPU or GPU memory, and moving them is part of the model’s runtime story. Source: `[14:25]` to `[16:39]`.
- **Views are cheap; copies are not.** Contiguity, strides, and aliasing determine whether an operation allocates new storage or just reinterprets old storage. Source: `[19:17]` to `[22:36]`.
- **Autograd is the engine behind the backwards pass.** The lecture shows that gradients are computed by PyTorch rather than hand-derived in practice. Source: `[50:01]` to `[58:19]`.
- **Activation checkpointing exists because storage is expensive.** If you can recompute activations, you can trade compute for memory. Source: `[1:13:39]` to `[1:14:11]`.

## Non-Obvious Insights
- The lecture is basically a warning against intuition by toy scale. At training scale, “just use the model” becomes “account for every byte and every FLOP.” Source: `[3:31]` to `[3:46]`.
- The FLOP story is incomplete. A component that is only a tiny fraction of FLOPs can still dominate runtime if it causes memory traffic. Source: `[13:23]` to `[14:59]`.
- bf16 is not “smaller float32.” It is a specific compromise: enough exponent range to behave well, enough savings to matter, and usually the default for training. Source: `[10:14]` to `[14:05]`.
- The practical memory limit for model size is often optimizer state, not the weights alone. That is why AdamW changes the ceiling so aggressively. Source: `[2:31]` to `[3:03]`, `[1:10:35]` to `[1:11:58]`.
- The lecture’s real engineering message is that optimization, numerics, and hardware layout are inseparable. You do not get a clean model/training boundary in practice. Source: `[57:14]` to `[59:25]`, `[1:12:16]` to `[1:12:34]`.

## Connections
- **To L01:** tokenization controls sequence length, which feeds directly into the compute and memory budget here.
- **To L03:** the architecture lecture turns this accounting into design choices: norms, activations, parallelism, and hyperparameters all show up as resource decisions.
- **To future systems work:** the lecture points forward to GPU architecture, memory movement, and tensor-core utilization as the next layer of detail.

## Referenced Papers
- **None explicitly named in the lecture.** The live content is implementation math, not literature review.

## Timestamps
- **0:28 - 3:25**: motivation, resource accounting examples, 70B training napkin math.
- **5:20 - 14:05**: tensors, memory sizing, float32 vs bf16.
- **17:36 - 23:21**: tensor internals, storage, views, contiguity.
- **50:01 - 59:25**: gradients, forward/backward cost, `6 x parameters x tokens`.
- **1:10:35 - 1:14:11**: optimizer memory and activation checkpointing.
- **1:15:19 - 1:18:56**: summary and why the lecture matters.

**Sources**
- Transcript: `/Users/otonashi/thinking/pratchett-os/tmp/research-001-l02-transcript.txt`
- Video: `https://www.youtube.com/watch?v=msHyYioAyNE`
