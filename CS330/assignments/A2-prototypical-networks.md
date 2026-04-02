# A2: Prototypical Networks

This is the clean half of the homework. No inner-loop adaptation, no gradient gymnastics. The whole model is the embedding space and the class means you build inside it.

## Problem Statement
- Train a few-shot classifier on Omniglot tasks.
- Use the support set to build one prototype per class.
- Classify query images by distance to those prototypes.
- Compare 5-way 1-shot and 5-way 5-shot training.
- Check whether the model still works when you test with more support examples than it saw during training.

## Key Techniques
- **Prototype construction.** For each class, average the support embeddings to get a class prototype.
- **Metric learning.** Use squared Euclidean distance from the query embedding to each prototype.
- **Softmax over negative distances.** Turn distances into logits and probabilities.
- **Query-set training.** Compute the loss on query examples, not the support set.
- **Task-level batching.** Treat each episode as a small classification problem with its own class IDs.

## Implementation Walkthrough
- `protonet.py` implements `ProtoNet.step`, which computes prototypes, logits, loss, and accuracy.
- The assignment uses 15 query examples per class per task.
- The course asks for validation query-accuracy curves, plus deeper metric checks:
  - are same-class support examples clustering tightly?
  - is the model generalizing to new tasks or overfitting?
- The comparison section checks 1-shot versus 5-shot training on 1-shot test tasks.
- The final test-time sweep increases `K` to see how gracefully the model absorbs extra support data.

## What You Learn
- A good embedding space can do most of the work.
- ProtoNets are simpler than MAML, but that simplicity is the point.
- Support/query separation is the training signal: support defines the class geometry, query drives the gradient.
- If the metric space is good, extra support examples at test time are easy to use.
- This is the course’s cleanest example of “learn the representation, not the per-task optimizer.”

## Sources
- [CS 330 Autumn 2022 Homework 2](https://cs330.stanford.edu/fall2022/materials/hw2_handout.pdf)
- [CS 330 Fall 2022 course schedule](https://cs330.stanford.edu/fall2022/index.html)
- [CS330 course mirror on GitHub](https://github.com/Andrew-Ng-s-number-one-fan/CS330-Deep-Multi-Task-and-Meta-Learning)
