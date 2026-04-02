# A2: Word2Vec Foundations and Training

## Problem Statement

Stanford's current public Assignment 2 frames word2vec as a math-and-optimization exercise: derive the gradients behind skip-gram, reason about negative sampling, then connect those ideas to neural network training tricks like Adam and dropout. The most useful public implementation reference is the 2021 assignment repo, where the same ideas are spelled out in code. Together, they turn word2vec from a slogan into an actual training pipeline.

## Key Techniques

- **Skip-gram objective**: predict outside words from a center word.
- **Naive softmax loss**: compute a full softmax over the vocabulary for one center-outside pair.
- **Negative sampling**: replace the expensive full-vocabulary update with one positive example plus sampled negatives.
- **Sigmoid-based binary terms**: score positive and negative pairs separately in the sampled objective.
- **Stochastic gradient descent**: update embedding matrices incrementally instead of solving a closed-form factorization.
- **Learning-rate annealing and checkpointing**: keep long training runs stable enough to finish.

## Implementation Walkthrough

The code path begins in `word2vec.py` with a scalar-or-vector `sigmoid(x)`, then immediately uses it inside two loss functions. `naiveSoftmaxLossAndGradient(...)` computes scores with `outsideVectors @ centerWordVec`, turns them into probabilities with a numerically stable softmax, applies cross-entropy loss against the one-hot true outside word, and derives gradients through the familiar `y_hat - y` term. In practice, this function is the cleanest place to understand how embedding learning is just multiclass classification with shared parameters.

`negSamplingLossAndGradient(...)` changes the computational shape. Instead of touching every vocabulary row, it samples `K` negative word indices, scores the true outside word with `sigmoid(u_o^T v_c)`, scores sampled negatives with `sigmoid(-u_k^T v_c)`, and accumulates the corresponding gradients. The important detail is that repeated negative samples must be counted multiple times. That is why the implementation updates `gradOutsideVecs[w_k]` with `+=` rather than overwriting it.

`skipgram(...)` is the composition layer. It takes one center word, loops over all outside words in the context window, calls either the naive-softmax or negative-sampling loss, sums the losses, and accumulates gradients into the center and outside embedding matrices. This is the moment where the local objective becomes a training example generator for the whole corpus.

Training itself is handled by `sgd.py` and `run.py`. `sgd.py` provides a plain but serious SGD loop: it restores saved parameters if asked, prints an exponentially smoothed loss, checkpoints every 5,000 iterations, and halves the step size every 20,000 iterations. `run.py` seeds the random generators, initializes separate input and output embedding tables, trains 10-dimensional vectors for 40,000 iterations on the Stanford Sentiment dataset with context size 5, and finally concatenates the learned input/output vectors for visualization.

That combination matters. The handout gives the derivations. The GitHub repo shows the actual moving parts you need in order to make the derivations train real vectors: stable softmax, correct gradient shapes, repeated-negative handling, checkpointed SGD, and a downstream visualization pass to inspect the learned geometry.

## What You Learn

- Word2vec is not mysterious; it is a structured classification objective over embedding tables.
- Negative sampling is the practical hack that makes training feasible without summing over the whole vocabulary every step.
- The split between center vectors and outside vectors is part of the model, not an implementation accident.
- Small optimization choices like annealing and checkpointing materially affect whether a long embedding run converges.
- Vector visualizations are useful sanity checks, but they are only downstream projections of the real learned space.

## Sources

- Stanford CS224N A2 handout: https://cs224n.stanford.edu/assignments_w25/a2.pdf
- Public 2021 implementation reference, `word2vec.py`: https://raw.githubusercontent.com/amanchadha/stanford-cs224n-assignments-2021/main/a2/word2vec.py
- Public 2021 implementation reference, `sgd.py`: https://raw.githubusercontent.com/amanchadha/stanford-cs224n-assignments-2021/main/a2/sgd.py
- Public 2021 training script, `run.py`: https://raw.githubusercontent.com/amanchadha/stanford-cs224n-assignments-2021/main/a2/run.py
