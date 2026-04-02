# CS330 Assignment 5: Advanced Meta-Learning Topics

## Problem Statement
This assignment moves away from the “just train the model” instinct and asks whether meta-learning is actually learning a transferable procedure or just memorizing tasks.

It has three parts:

- **Memorization analysis.** Decide whether complete memorization can happen in several meta-learning task distributions.
- **Bayesian meta-learning.** Derive an ELBO for a hierarchical model that uses task metadata as side information.
- **Scaling study.** Compare the compute cost of MAML and Evolution Strategies on a toy sinusoid benchmark.

The assignment is mostly conceptual, but it is not hand-wavy. The questions force you to reason about task distributions, hidden variables, and the real runtime cost of inner-loop adaptation.

## Key Techniques
- **Task-distribution reasoning.** The first problem asks whether a meta-learner can ignore the support set and still score perfectly on the training tasks.
- **Hierarchical Bayesian modeling.** The second problem introduces two latent variables and observed task metadata, then asks for a plate diagram and ELBO.
- **Amortized inference.** The variational setup uses learned recognition models rather than per-task optimization from scratch.
- **Unrolled optimization.** MAML backpropagates through inner-loop gradient steps, which makes memory use grow with the number of steps.
- **Black-box optimization.** ES estimates meta-gradients without the same unrolled backprop cost, so the runtime/memory profile is very different.
- **Compute profiling.** The assignment treats GPU memory and runtime as first-class outputs, not side effects.

## Implementation Walkthrough
1. **Work through the memorization cases one by one.**
   - For the basis-function regression setting, decide whether task identity can be reconstructed from the query set alone.
   - For medical image classification, think about whether disease-specific structure makes memorization plausible.
   - For robotic grasping, separate the training signal from the task metadata hidden in the scene.

2. **Draw the Bayesian meta-learning graph.**
   - Include `theta`, `phi1`, `phi2`, task metadata `m`, datapoints `x`, `y`, and the task/data plates.
   - Make the observable and latent variables explicit before attempting the ELBO.

3. **Derive the ELBO cleanly.**
   - Write the joint distribution with `p(phi1)`, `p(phi2 | phi1, m)`, and `p(y | phi2, x)`.
   - Introduce compact notation for `X = x1:N` and `Y = y1:N`.
   - Apply Jensen in the usual variational way until the lower bound is explicit.

4. **Run the scaling experiment.**
   - Use the provided scripts to compare MAML and ES on the sinusoid regression benchmark.
   - Sweep the number of inner-loop steps and inspect runtime plus allocated GPU memory.
   - Record which configuration makes the gap between the two methods most obvious.

5. **Write the interpretation, not just the numbers.**
   - The point is not that one algorithm is universally better.
   - The point is that unrolled gradient methods and black-box methods pay different costs for adaptation.

## What You Learn
- Meta-learning can fail by memorizing, not just by underfitting.
- Metadata is naturally expressed through hierarchical latent structure.
- The ELBO is the bridge between probabilistic modeling and practical training objectives.
- MAML and ES optimize the same broad goal but make very different compute tradeoffs.
- Algorithm choice matters as much as benchmark score once inner-loop depth and memory pressure enter the picture.

## Sources
- Official homework PDF: https://cs330.stanford.edu/materials/hw4.pdf
- Public course mirror: https://github.com/Andrew-Ng-s-number-one-fan/CS330-Deep-Multi-Task-and-Meta-Learning
