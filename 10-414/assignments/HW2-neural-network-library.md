# 10-714 Homework 2: Neural Network Library

## Problem Statement
Homework 2 turns Needle from an autodiff engine into an actual neural network library.

The assignment adds:
- parameter initialization
- reusable modules
- optimizers
- datasets, transforms, and dataloaders
- an MLP ResNet training loop on MNIST

This is the point where framework work starts to look like product work. The model code is only one slice of the job; the rest is state, stability, and data plumbing.

## Key Techniques
- **Initialization matters.** Xavier and Kaiming schemes keep signal scale under control as depth grows.
- **Parameter registration.** `Parameter` wrappers and `Module` state traversal make optimizers discoverable without hand-maintained lists.
- **Numerically stable losses.** `LogSumExp`, `LogSoftmax`, and `SoftmaxLoss` avoid the overflow that a naive exponentiation path would hit immediately.
- **Layer state.** `BatchNorm1d`, `LayerNorm1d`, `Dropout`, and `Residual` all require the module to know whether it is training or evaluating.
- **Optimizer state.** SGD with momentum and Adam both keep hidden state across steps, so the update rule is no longer a stateless function.
- **Data iteration.** The `Dataset` / `DataLoader` split is the usual mini-batch abstraction, but now it is part of the library rather than user code.

## Implementation Walkthrough
1. Write the initializer functions in `python/needle/init/init_initializers.py`. The notebook asks for Xavier uniform/normal and Kaiming uniform/normal.
2. Fill out core layers in `python/needle/nn/nn_basic.py`: `Linear`, `ReLU`, `Sequential`, `LogSumExp`, `LogSoftmax`, `SoftmaxLoss`, `LayerNorm1d`, `Flatten`, `BatchNorm1d`, `Dropout`, and `Residual`.
3. Implement optimizer steps in `python/needle/optim.py` for `SGD` and `Adam`. The notebook warns explicitly about not mutating gradients in place and about memory growth from accidental graph construction.
4. Implement transforms and data access in `python/needle/data/data_transforms.py`, `python/needle/data/data_basic.py`, and `python/needle/data/datasets/mnist_dataset.py`.
5. Build the residual MLP in `apps/mlp_resnet.py` with `ResidualBlock`, `MLPResNet`, `epoch`, and `train_mnist`.
6. Keep parameter initialization order consistent with the notebook guidance. The tests were generated against a specific order, so weights-first versus bias-first is not a cosmetic detail here.

## What You Learn
- A neural network library is mostly contracts: shapes, state, and stability.
- Initializers are part of model quality, not just boilerplate.
- Training loops are a library feature, because models and data loaders have to agree on mode, batching, and dtype.
- Residual connections are what make the MLP deep enough to matter.
- The difference between a toy autograd engine and a usable framework is all the glue around the graph.

## Sources
- `https://raw.githubusercontent.com/dlsyscourse/hw2/main/hw2.ipynb`
- `https://github.com/dlsyscourse/hw2`
- `https://dlsyscourse.org/`
