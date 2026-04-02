# 10-714 Homework 4 Extension: Transformers in Needle

## Problem Statement
The transformer extension replaces recurrence with masked attention and a learned positional signal. Needle has to build a decoder-only language model from batched matmul, normalization, dropout, and residual blocks.

The job is not to clone a paper diagram. The job is to make causal attention, prenorm residuals, and position-aware embeddings work together in the same model.

## Key Techniques
- **Masked multi-head attention.** `MultiHeadAttention` computes `softmax(QK^T / sqrt(D)) V` with causal masking and dropout.
- **Prenorm projections.** `AttentionLayer` normalizes queries, keys, and values before projecting them into heads.
- **Head reshaping.** The attention path has to move between `B x T x (H D)` and `B x H x T x D` cleanly.
- **Residual MLP block.** `TransformerLayer` combines attention with a two-layer feedforward path and dropout on the residual branch.
- **Learned positional embeddings.** `Embedding` provides trainable position ids so the model knows token order without sinusoidal tricks.

## Implementation Walkthrough
1. Implement the base attention activation in `MultiHeadAttention.forward`, including masking and dropout.
2. Wrap it in `AttentionLayer` so self-attention and cross-attention both flow through the same head machinery.
3. Build `TransformerLayer` with prenorm, residual connections, and the feedforward block.
4. Stack the layers in `Transformer` and add learned positional embeddings before the first block.
5. Wire the transformer into `LanguageModel` and run the PTB language-model path end to end.
6. Verify the causal mask and output shapes first; everything else depends on those two checks.

## What You Learn
- Attention is batched linear algebra with a mask in the middle.
- Positional information has to be injected explicitly once recurrence is gone.
- Prenorm is the stability trick that keeps the residual stack trainable.
- Decoder-only transformers are still just next-token language models, with a different internal machine.

## Sources
- `https://raw.githubusercontent.com/dlsyscourse/hw4_extra/main/hw4_extra.ipynb`
- `https://github.com/dlsyscourse/hw4_extra`
- `https://dlsyscourse.org/assignments/`
