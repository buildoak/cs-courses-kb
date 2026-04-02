# 10-714 Homework 1: Automatic Differentiation

## Problem Statement
This is the first Needle homework. The assignment builds the autodiff core from scratch, then uses it to re-implement the simple two-layer MNIST network from HW0.

The work is split into two halves:
- build a computation graph engine with forward ops, backward rules, topological sort, and reverse-mode AD
- reuse that engine in `apps/simple_ml.py` for softmax loss and SGD on a ReLU MLP

The point is not "write a few derivatives." The point is to make gradients a first-class graph object, so the rest of Needle can be expressed on top of it.

## Key Techniques
- **Compute / gradient split.** Forward `compute()` methods operate on raw NDArray data; `gradient()` methods stay inside Needle ops so higher-order derivatives still work.
- **Reverse-mode AD.** The homework is organized around a scalar loss flowing backward through a graph, which is why reverse mode is the right tool.
- **Topological traversal.** Gradients are accumulated in reverse topological order, so each node sees all downstream contributions before it propagates upstream.
- **Shape-aware derivatives.** Broadcast, summation, reshape, and transpose are the cases where the algebra is easy but the dimensions are not.
- **Subgradient ReLU.** The homework fixes the derivative at `x = 0` to `0`, which keeps the implementation deterministic and testable.
- **Tensor-based training loop.** The `nn_epoch` solution uses `Tensor` weights with `.backward()`, then updates them in NumPy and re-wraps them.

## Implementation Walkthrough
1. Implement forward ops in `python/needle/ops/ops_mathematic.py`. The notebook points to `PowerScalar`, `EWiseDiv`, `DivScalar`, `MatMul`, `Summation`, `BroadcastTo`, `Reshape`, `Negate`, `Transpose`, `Log`, `Exp`, and `EWisePow`.
2. Implement backward rules for the same operators using Needle ops only. The important cases are `Summation` and `BroadcastTo`, because they force you to track the original input shape during the gradient pass.
3. Implement graph utilities in `python/needle/autograd.py`: `find_topo_sort`, `topo_sort_dfs`, and `compute_gradient_of_variables`.
4. Add the reverse-mode plumbing so `Tensor.backward()` can populate `.grad` fields on inputs.
5. Implement `softmax_loss()` in `apps/simple_ml.py` for one-hot labels and batched logits.
6. Implement `ReLU` and `nn_epoch()` so the old two-layer MNIST network can train with the new autodiff engine.
7. Verify with the provided gradient checks before trusting the training loop. The notebook explicitly points at `tests/test_autograd.py` and `tests/test_autograd_hw.py` for this.

## What You Learn
- Autodiff is graph bookkeeping plus the chain rule.
- Forward correctness and backward correctness are separate problems.
- Broadcasting is cheap on the way forward and annoying on the way back.
- Once the gradient engine exists, training code gets smaller, not larger.
- Reverse mode is the reason deep learning frameworks can make scalar losses cheap.

## Sources
- `https://raw.githubusercontent.com/dlsyscourse/hw1/main/hw1.ipynb`
- `https://github.com/dlsyscourse/hw1`
- `https://dlsyscourse.org/`
