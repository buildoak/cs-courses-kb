# A1: Multitask Training for Recommender Systems

This warmup assignment is not about elegance. It is about forcing one model to serve two objectives and seeing where sharing helps, where it hurts, and how much the loss weights steer the whole thing.

## Problem Statement
- Build a multi-task recommender on MovieLens 100K.
- Predict two outputs from the same user-item pair:
  - whether the user would watch the movie
  - what score the user would give it
- Use matrix factorization for the implicit-feedback side and a small MLP for the rating side.
- Compare shared embeddings against separate embeddings, then vary the task weights.
- Evaluate with both ranking and regression metrics, not one blended number.

## Key Techniques
- **Matrix factorization for likelihood prediction.** Learn user and item embeddings, then score a pair with a dot product plus user and item bias terms.
- **BPR loss.** Train the implicit-feedback branch with Bayesian Personalized Ranking so positive items outrank sampled negatives.
- **Regression head on interaction features.** Feed `[u, q, u * q]` into an MLP to predict explicit ratings.
- **Shared vs separate representation learning.** Reuse the same latent vectors for both tasks, or split them apart and compare.
- **Loss weighting.** Tune `λF` and `λR` to see whether ranking or regression should dominate.

## Implementation Walkthrough
- `models.py` defines `ScaledEmbedding` layers for users and items and `ZeroEmbedding` layers for the bias terms.
- The MLP uses an input width of 96 when `embedding_dim = 32`, because the model concatenates `u`, `q`, and `u * q`.
- `forward` returns both outputs in one pass:
  - watch probability from the factorization branch
  - predicted rating from the regression branch
- The training script logs BPR loss, MSE loss, test MSE, and MRR to TensorBoard.
- The write-up asks for four runs:
  - shared embeddings with `λF = 0.99`, `λR = 0.01`
  - shared embeddings with `λF = 0.5`, `λR = 0.5`
  - separate embeddings with `λF = 0.5`, `λR = 0.5`
  - separate embeddings with `λF = 0.99`, `λR = 0.01`

## What You Learn
- Multi-task sharing is a bet, not a free win.
- If the two objectives align, shared embeddings can reduce waste and improve generalization.
- If the tasks fight, sharing just creates interference.
- Ranking quality and rating quality are different problems. MRR and MSE expose different failure modes.
- The loss weights are not decoration. They decide which task owns the representation.

## Sources
- [CS 330 Autumn 2022 Warmup Homework 0](https://cs330.stanford.edu/fall2022/materials/CS330_HW0.pdf)
- [CS 330 Fall 2022 course schedule](https://cs330.stanford.edu/fall2022/index.html)
- [CS330 course mirror on GitHub](https://github.com/Andrew-Ng-s-number-one-fan/CS330-Deep-Multi-Task-and-Meta-Learning)
