# CS336 Assignment 1: Transformer LM and Training Loop

## Problem Statement
After the tokenizer, Assignment 1 moves up one level and asks for a complete from-scratch language model stack: pre-norm Transformer blocks, LM forward pass, cross-entropy, AdamW, cosine warmup scheduling, gradient clipping, data loading, checkpointing, and a training script that can actually run experiments.

The assignment is intentionally restrictive. The handout forbids leaning on most of `torch.nn.functional` and `torch.optim`, so the student has to rebuild the core numerical pieces and then prove they match the reference implementation through tests and state-dict contracts.

## Key Techniques
- **Pre-norm Transformer design.** The block order is RMSNorm -> sublayer -> dropout -> residual, with a final RMSNorm before the LM head.
- **Numerically stable tensor ops.** Softmax and cross-entropy both require the usual "subtract max" trick to avoid overflow.
- **Batched attention implementation.** Multi-head self-attention should compute Q, K, and V projections in batched matrix multiplies and apply a causal mask so future tokens stay hidden.
- **State-aware optimization.** AdamW tracks first and second moments per parameter, while cosine scheduling with warmup controls learning-rate shape over training.
- **Training infrastructure, not just model code.** `get_batch`, checkpoint save/load, memory-mapped datasets, and a configurable training script are all part of the assignment surface.

## Implementation Walkthrough
1. **Start with the scalar and vector primitives.** RMSNorm, GELU, softmax, and cross-entropy are the bedrock. The tests isolate each of them because any small numerical bug propagates upward into the full model.
2. **Assemble the Transformer block from those primitives.** The adapter contract spells out the expected state-dict keys for attention projections, FFN weights, and normalization gains. Matching those shapes and names makes it possible to compare the student block against a reference forward pass.
3. **Build the full LM around embeddings and stacked blocks.** The full model adds token embeddings, position embeddings, repeated pre-norm blocks, a final RMSNorm, and an output projection over the vocabulary. The truncated-input test ensures the forward pass works when sequence length is smaller than the model context window.
4. **Implement optimization as real optimizer code, not a helper function.** AdamW is expected as a `torch.optim.Optimizer` subclass, which means per-parameter state, in-place updates, and compatibility with optimizer state serialization.
5. **Wire up training utilities that survive interruption.** `get_batch` samples random contiguous spans and shifted targets; checkpointing saves model state, optimizer state, and iteration count; the training script is expected to support memmapped datasets, periodic logging, and resumability.
6. **Close the loop with generation and experiments.** Once the training stack exists, the same model is used for decoding, TinyStories runs, OpenWebText experiments, and ablations. The assignment is teaching that "model implementation" and "training implementation" are inseparable in practice.

## What You Learn
- Transformer equations are the easy part. Getting shape discipline, masking, dropout placement, and state layout right is the real work.
- Training code is mostly about preserving invariants: numerical stability, optimizer state continuity, and reproducible data/checkpoint semantics.
- A clean decomposition pays off. The same small primitives get reused in the LM, the optimizer, the training loop, and later systems work.
- By the end of Assignment 1, you do not just understand Transformer math. You understand what must exist before a model can actually train.

## Sources
- Handout: `https://github.com/stanford-cs336/spring2024-assignment1-basics/blob/main/cs336_spring2024_assignment1_basics.pdf` (Assignment 1, sections 3-6)
- Repo overview: `https://github.com/stanford-cs336/spring2024-assignment1-basics`
- Adapter contract: `/Users/otonashi/thinking/pratchett-os/tmp/research-008-a1/tests/adapters.py`
- Model tests: `/Users/otonashi/thinking/pratchett-os/tmp/research-008-a1/tests/test_model.py`
- NN utility tests: `/Users/otonashi/thinking/pratchett-os/tmp/research-008-a1/tests/test_nn_utils.py`
- Optimizer tests: `/Users/otonashi/thinking/pratchett-os/tmp/research-008-a1/tests/test_optimizer.py`
- Data loader test: `/Users/otonashi/thinking/pratchett-os/tmp/research-008-a1/tests/test_data.py`
- Checkpoint tests: `/Users/otonashi/thinking/pratchett-os/tmp/research-008-a1/tests/test_serialization.py`
