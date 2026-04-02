# CS236: Normalizing Flows

## Problem Statement

Normalizing flows try to get both things at once: exact likelihoods and flexible generative modeling. Instead of hiding the data behind a hard latent inference problem, they build an invertible map between simple noise and data.

The assignment-shaped job is to make that invertible map real in code, compute the log-density correctly, and keep the Jacobian accounting honest.

## Key Techniques

- **Change of variables:** transform a tractable base density into a complex data density with an invertible function.
- **Jacobian determinant tracking:** every transform contributes a log-determinant term to the likelihood.
- **Invertible architecture design:** the transformation must be bijective, differentiable, and cheap to evaluate.
- **Coupling layers:** split the input and transform one part conditioned on the other.
- **Autoregressive flows:** choose the forward or reverse direction depending on whether sampling or likelihood is the main concern.

## Implementation Walkthrough

1. **Start from a base distribution.**
   - Usually `z ~ N(0, I)`.
   - This is the easy density you can sample from and evaluate exactly.

2. **Stack invertible transforms.**
   - Planar flow gives a simple non-linear invertible step.
   - NICE and RealNVP use coupling layers.
   - MAF and IAF trade off sampling speed against likelihood speed.

3. **Accumulate the log-density.**
   - Each layer contributes `log |det J|`.
   - The final likelihood is the base log-density plus all log-det corrections.

4. **Watch the trade-offs.**
   - Some flows sample fast but evaluate slowly.
   - Others evaluate fast but sample slowly.
   - The architecture choice is not cosmetic; it determines the runtime profile of the whole model.

5. **Keep the transform invertible by construction.**
   - If the inverse is not tractable, the model stops being a flow.
   - If the Jacobian is too expensive, the model stops being practical.

6. **Use the flow as a density model, not just a fancy layer stack.**
   - The point is exact likelihood, not just expressive features.
   - If the log-det bookkeeping is wrong, the whole model is wrong.

## What You Learn

- Why exact likelihoods are possible without abandoning neural nets.
- Why invertibility is the real constraint in flow design.
- Why sampling and likelihood evaluation often pull in opposite directions.
- Why Jacobian math is not decoration in probabilistic code.

## Sources

- `https://deepgenerativemodels.github.io/notes/flow/`
- `https://deepgenerativemodels.github.io/syllabus.html`

