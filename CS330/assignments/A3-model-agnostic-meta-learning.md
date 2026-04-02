# A3: Model-Agnostic Meta-Learning

MAML is the other half of the assignment pair: instead of building a static metric space, it learns an initialization that can move fast when a new task arrives.

## Problem Statement
- Meta-learn parameters that adapt quickly on new 5-way 1-shot Omniglot tasks.
- Implement both the inner loop and the outer meta-update.
- Measure post-adaptation query accuracy, not just raw pre-adaptation accuracy.
- Sweep the inner learning rate, the number of inner steps, and whether the inner learning rates are learned.
- Compare MAML against ProtoNets when test tasks have more support examples.

## Key Techniques
- **Support/query split.** Use support data for adaptation and query data for the meta-objective.
- **Inner loop optimization.** Run gradient descent on the support set to get task-specific parameters.
- **Outer loop optimization.** Update the shared initialization using query loss after adaptation.
- **Higher-order gradients.** `autograd.grad` is the clean way to implement the differentiable inner loop.
- **Learned inner learning rates.** Give each parameter group its own step size instead of one scalar for everything.

## Implementation Walkthrough
- `maml.py` asks you to implement `inner_loop` and `outer_step`.
- The baseline setup uses:
  - 1 inner step
  - inner learning rate `0.4`
  - 15 query examples per class
- The analysis section reads the logged pre/post-adaptation accuracies and asks what they say about task sampling and adaptation quality.
- The experiment section compares:
  - inner lr `0.4` vs `0.04`
  - 1 inner step vs 5 inner steps
  - learned vs fixed inner learning rates
- The final comparison checks whether the trained MAML model can use extra support examples at test time without having been trained for that exact shot count.

## What You Learn
- MAML is not “a classifier with more steps.” It is an initialization search problem.
- Pre-adaptation metrics tell you whether the task distribution is making sense.
- Post-adaptation metrics tell you whether the initialization is actually useful.
- The inner learning rate is a real architectural choice, not a hyperparameter footnote.
- MAML and ProtoNets solve the same high-level problem from opposite ends: one learns to move fast, the other learns to stay geometrically organized.

## Sources
- [CS 330 Autumn 2022 Homework 2](https://cs330.stanford.edu/fall2022/materials/hw2_handout.pdf)
- [CS 330 Fall 2022 course schedule](https://cs330.stanford.edu/fall2022/index.html)
- [CS330 course mirror on GitHub](https://github.com/Andrew-Ng-s-number-one-fan/CS330-Deep-Multi-Task-and-Meta-Learning)
