# 10-714 Homework 4: Training Pipelines and Optimizers

## Problem Statement
Homework 4 turns Needle into a training stack that can actually chew through CIFAR-10. The assignment starts with backend layout ops, then pushes into convolution, dataset plumbing, ResNet9, and the training loop that ties all of it together.

The real problem is not "add one more layer." The problem is to keep layout, device placement, module state, and optimizer state aligned so the same code path can run without shape bugs or silent copies.

## Key Techniques
- **View-based array ops.** `reshape`, `permute`, `broadcast_to`, and `__getitem__` stay cheap by reusing storage instead of copying data.
- **Convolution from primitives.** `pad`, `flip`, `dilate`, and `Conv` turn Needle's array backend into a vision backend.
- **Dataset contract.** `CIFAR10Dataset` must return tensors in `(3, 32, 32)` order so the model and backend agree on layout.
- **NCHW in the model.** `BatchNorm2d` and `ResNet9` expect a channel-first path, so the notebook explicitly switches away from the older HW2 assumptions.
- **Stateful optimizers.** The notebook tells you to carry over `python/needle/optim.py` from previous homework; the optimizer is reused, but the training stack around it is new.
- **End-to-end verification.** `train_cifar10` and `evaluate_cifar10` are the final proof that the model, data, and backend are actually compatible.

## Implementation Walkthrough
1. Finish the NDArray layout methods and backend kernels so Needle can represent views without copying.
2. Implement the convolution path in `ops_mathematic.py` and wrap it in `nn.Conv`.
3. Add CIFAR-10 loading in `python/needle/data/datasets/cifar10_dataset.py` and keep the data transforms consistent with earlier homework.
4. Build `ResNet9` in `apps/models.py`, including the flattening step before the final linear layer.
5. Reuse the existing optimizer code from earlier homework and wire the CIFAR-10 training and evaluation helpers in `apps/simple_ml.py`.
6. Run a small training sanity check and verify the parameter count and early loss behavior before trusting the full run.

## What You Learn
- Training code fails when the invariants are wrong, not when the math is fancy.
- Convolution is a backend and layout problem before it is a model problem.
- Device placement has to be explicit once a model has nested modules.
- A framework becomes real when the full train/eval path works on a dataset, not when one operator passes a unit test.

## Sources
- `https://raw.githubusercontent.com/dlsyscourse/hw4/main/hw4.ipynb`
- `https://github.com/dlsyscourse/hw4`
- `https://dlsyscourse.org/assignments/`
