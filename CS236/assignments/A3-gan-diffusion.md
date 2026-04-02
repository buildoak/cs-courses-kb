# CS236: GANs and Diffusion

## Problem Statement

CS236 uses GANs to break out of the likelihood-first mindset. The generator does not try to explain every datapoint by density; it tries to fool a discriminator that learns the boundary between real and fake samples.

This is also the part of the course where the schedule turns toward score-based and diffusion models. The adversarial block is the bridge: once you stop optimizing log-likelihood directly, you need another way to push samples toward the data manifold.

## Key Techniques

- **Minimax training:** generator and discriminator play opposite roles.
- **Optimal discriminator view:** the discriminator estimates the likelihood ratio between data and model samples.
- **Non-saturating generator loss:** avoids the vanishing-gradient problem of the original minimax objective.
- **f-GAN / WGAN / WGAN-GP variants:** different ways to make adversarial training more stable or more flexible.
- **Conditional generation:** feed labels into both generator and discriminator when you want class-controlled outputs.
- **Score-based and diffusion follow-up:** the course moves from GANs into score-based diffusion models as the next generative family.

## Implementation Walkthrough

1. **Build two networks.**
   - Generator `G_theta(z)` maps noise to samples.
   - Discriminator `D_phi(x)` scores real versus fake.

2. **Alternate updates.**
   - Update `D` on real data and generated samples.
   - Update `G` to increase the discriminator score on its outputs.

3. **Use the non-saturating loss.**
   - The original minimax generator loss can stall when the discriminator gets too good.
   - The non-saturating variant gives stronger gradients early in training.

4. **Add conditioning if labels matter.**
   - Concatenate a one-hot label to the generator input.
   - Let the discriminator see both the sample and the label.

5. **Stabilize if training goes sideways.**
   - WGAN-GP adds a gradient penalty to keep the discriminator well-behaved.
   - f-GAN and CycleGAN show how the adversarial template gets reused for other tasks.

6. **Treat diffusion as the next block, not a side note.**
   - The 2023 CS236 syllabus moves from GANs into score-based diffusion and discrete diffusion immediately afterward.
   - That matters because it shows the course arc: adversarial learning is one answer to the sample-quality problem, diffusion is the next one.

## What You Learn

- Why GANs can produce sharp samples without a likelihood objective.
- Why adversarial training is unstable and why the loss choice matters.
- Why discriminator design is half the model.
- Why score-based diffusion appears naturally after GANs in a modern generative-models course.

## Sources

- `https://deepgenerativemodels.github.io/notes/gan/`
- `https://deepgenerativemodels.github.io/syllabus.html`

