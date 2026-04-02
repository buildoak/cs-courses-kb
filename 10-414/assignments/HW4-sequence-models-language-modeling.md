# 10-714 Homework 4: Sequence Models, RNNs, LSTMs, and Language Modeling

## Problem Statement
Once the CNN path works, homework 4 pushes Needle into time. The model has to carry hidden state across steps, batch the Penn Treebank corpus into time-major slices, and turn that stream into word-level prediction.

This is where the framework stops treating tensors as independent samples and starts treating them as a sequence with memory.

## Key Techniques
- **Stateful recurrence.** `RNNCell`, `RNN`, `LSTMCell`, and `LSTM` keep hidden state alive across time steps instead of pretending every token is isolated.
- **Stack / split over indexing.** The notebook uses `stack` and `split` to move between per-step and batched sequence forms without depending on unavailable tensor indexing tricks.
- **PTB data layout.** `Dictionary`, `Corpus`, `tokenize`, `batchify`, and `get_batch` turn a text stream into training-ready mini-batches.
- **Embedding plus decoder.** The language model is the standard pattern: `Embedding` -> sequence core -> linear head.
- **Training helpers.** `epoch_general_ptb`, `train_ptb`, and `evaluate_ptb` make the sequence model trainable and measurable.

## Implementation Walkthrough
1. Implement the recurrent modules in `python/needle/nn/nn_sequence.py`.
2. Add the Penn Treebank helpers in `python/needle/data/datasets/ptb_dataset.py`.
3. Shape the sequence batches with `batchify` and `get_batch` so time and batch dimensions stay readable.
4. Hook the recurrent blocks into `apps/models.py` through the `LanguageModel` class.
5. Finish the PTB training and evaluation loops in `apps/simple_ml.py`.
6. Check that the model can run forward and backward on real text before trusting the final loss numbers.

## What You Learn
- Sequence models are state machines with learnable transitions.
- The hard part is not the gate equations; it is the tensor plumbing around them.
- LSTM gates are the fix for gradients that would otherwise disappear in long sequences.
- Language modeling is mostly batching discipline and state carry.

## Sources
- `https://raw.githubusercontent.com/dlsyscourse/hw4/main/hw4.ipynb`
- `https://github.com/dlsyscourse/hw4`
- `https://dlsyscourse.org/assignments/`
