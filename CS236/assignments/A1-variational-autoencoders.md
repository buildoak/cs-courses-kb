# CS236: Variational Autoencoders

## Problem Statement

CS236 uses VAEs to answer a simple but annoying question: how do you train a latent-variable generative model when the marginal likelihood is intractable? The model wants to explain data with hidden structure, but exact posterior inference is too expensive to do by brute force.

The core assignment-style job is to turn the math into code: encode an input into a Gaussian posterior, sample a latent variable without breaking gradients, decode back to data space, and optimize the ELBO instead of the raw likelihood.

## Key Techniques

- **Latent-variable factorization:** model the joint as `p(x, z) = p(x | z) p(z)`.
- **ELBO training:** maximize a tractable lower bound on `log p(x)` instead of the marginal likelihood directly.
- **Reparameterization trick:** sample `z = mu + sigma * epsilon` so gradients flow through the sample path.
- **Amortized inference:** use an encoder network `q_phi(z | x)` instead of solving a new posterior for every datapoint.
- **KL regularization:** keep the learned posterior near the prior so sampling stays well-behaved.

## Implementation Walkthrough

1. **Build the encoder.**
   - Map `x` to posterior parameters `mu` and `logvar`.
   - Keep the output diagonal-Gaussian so sampling and KL computation stay simple.

2. **Sample with reparameterization.**
   - Draw `epsilon ~ N(0, I)`.
   - Form `z = mu + exp(0.5 * logvar) * epsilon`.
   - This is the part that keeps backpropagation intact.

3. **Build the decoder.**
   - Map `z` back to reconstruction parameters.
   - For binary images, use Bernoulli output.
   - For continuous data, switch to a Gaussian decoder.

4. **Compute the objective.**
   - Reconstruction term: how well the decoder matches the input.
   - KL term: how far the approximate posterior drifts from the prior.
   - Sum them as the negative ELBO if your optimizer minimizes loss.

5. **Check samples, not just loss.**
   - Loss can go down while samples stay ugly.
   - Sample a grid from the prior and inspect whether the latent space actually learned structure.

6. **Keep the pattern reusable.**
   - Once the encoder-decoder-KL loop is stable, the same pattern extends to richer latent-variable variants.

## What You Learn

- Why latent-variable models need approximate inference.
- Why the ELBO is the practical objective, not a theoretical footnote.
- Why the reparameterization trick is the difference between a paper and runnable code.
- Why likelihood and sample quality are related but not identical.

## Sources

- `https://deepgenerativemodels.github.io/notes/vae/`
- `https://deepgenerativemodels.github.io/syllabus.html`

