# 10-714 Homework 4: Sequence Models, RNNs, and LSTMs

## Problem Statement
The public sequence homework in the `dlsyscourse` repo is labeled `hw4`; this article fills the ticket's hw3 sequence-model slot with the actual sequence/LSTM notebook content.

This assignment finishes the Needle stack by pushing it into vision and language modeling:
- add NDArray utilities and convolution support
- build a ResNet9-style CIFAR-10 classifier
- implement RNN and LSTM sequence modules
- train a word-level language model on Penn Treebank

The important shift is temporal. The model no longer maps one input to one output in isolation; it now carries state across time steps and across batches.

## Key Techniques
- **Backend primitives.** `flip` and `pad` make convolution and its backward pass manageable in the NDArray backend.
- **Convolution by composition.** `Flip`, `Dilate`, `UnDilate`, and `Conv` are built as Needle ops, then wrapped in `nn.Conv`.
- **Residual vision stack.** `ResNet9` uses the convolutional backend to train a practical CIFAR-10 classifier without needing a full external framework.
- **Stateful recurrence.** `RNNCell`, `RNN`, `LSTMCell`, and `LSTM` carry hidden state forward through time, which is the whole point of sequence modeling.
- **Stack / split instead of indexing.** The notebook warns that tensor indexing is not available, so sequence code uses `stack` and `split` to move between per-step and batched forms.
- **Language-model batching.** The Penn Treebank helpers `Dictionary`, `Corpus`, `batchify`, and `get_batch` turn a long token stream into efficient time-major mini-batches.
- **Embedding as lookup.** The final language model is embedding + sequence model + linear head, which is the standard word-level LM pattern.

## Implementation Walkthrough
1. Finish the remaining NDArray helpers in `python/needle/backend_ndarray/ndarray.py`, especially `flip` and `pad`.
2. Implement the convolution pipeline in `python/needle/ops/ops_mathematic.py` and `python/needle/nn/nn_conv.py`.
3. Build `ResNet9` in `apps/models.py`, then wire the CIFAR-10 training helpers in `apps/simple_ml.py`.
4. Implement `RNNCell` and `RNN` in `python/needle/nn/nn_sequence.py`, using the tanh/relu recurrence described in the notebook.
5. Implement `Sigmoid`, `LSTMCell`, and `LSTM` in the same module. The gate equations are the real meat of the homework.
6. Implement Penn Treebank data helpers in `python/needle/data/datasets/ptb_dataset.py`.
7. Finish the language-model training path in `apps/models.py` and `apps/simple_ml.py` with `Embedding`, `LanguageModel`, `epoch_general_ptb`, `train_ptb`, and `evaluate_ptb`.

## What You Learn
- Sequence models are just recurrent state machines with learnable transitions.
- The unrolled time dimension is a graph problem, not a special case.
- `stack` and `split` are enough to express a surprising amount of recurrent plumbing.
- Language modeling is mostly data layout: tokenization, batching, and target shifting.
- RNNs are the simple baseline; LSTMs are the answer to "how do we stop recurrence from forgetting everything."

## Sources
- `https://raw.githubusercontent.com/dlsyscourse/hw4/main/hw4.ipynb`
- `https://github.com/dlsyscourse/hw4`
- `https://dlsyscourse.org/`
