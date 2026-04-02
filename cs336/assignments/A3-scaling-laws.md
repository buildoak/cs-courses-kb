# CS336 Assignment 3: Scaling Laws

## Problem Statement
Assignment 3 turns model scaling into an experimental design problem. You are given a fixed training budget and asked to predict the compute-optimal language model: not just how many parameters it should have, but also how many tokens it should train on, which architectural hyperparameters to use, and what final training loss to expect.

The assignment has two phases. First, you reproduce an IsoFLOPs-style scaling-law fit on synthetic runs so you can validate the fitting procedure in a controlled setting. Then you move to the real task: spend a limited exploration budget of `2e18` FLOPs through a training API, fit a scaling law from those experimental points, and extrapolate the best model and training setup for a larger `1e19` FLOPs run.

What makes this interesting is the constraint: brute force is impossible. The assignment is really about how to spend limited experiments well, how to fit stable power laws from noisy runs, and how to translate a predicted parameter count into an actual Transformer configuration.

## Key Techniques
- **IsoFLOPs profiles.** Hold compute fixed and vary model size. Since training compute is approximated as `C = 6ND`, changing parameter count `N` automatically changes token budget `D = C / (6N)`.
- **Compute-optimal frontier fitting.** For each compute budget, find the run with lowest loss, then fit power laws for `N_opt(C)` and `D_opt(C)` on log-log axes.
- **Synthetic warm-up before live querying.** The handout starts with a JSON file of completed runs so you can validate grouping, minimization, and extrapolation before touching the limited API budget.
- **Budgeted experimental design.** The live API enforces a hard FLOPs cap, so you need a search policy: broad sweeps to find promising regions, then finer sweeps around the likely optimum.
- **Architecture-to-parameter mapping.** The handout gives the approximation `12 * n_layer * d_model^2` for non-embedding parameters, which lets you turn a target parameter count into a plausible model shape.
- **Hyperparameter coupling.** Model size alone is not enough. The API also requires `d_model`, `num_layers`, `num_heads`, `batch_size`, `learning_rate`, and `train_flops`, so the scaling-law estimate has to be converted into a runnable training configuration.

## Implementation Walkthrough
1. **Rebuild the IsoFLOPs workflow on the provided synthetic runs.** Load the JSON runs, group them by compute budget, and identify the minimum-loss run inside each budget bucket. Those minima become the empirical compute-optimal points for model size.
2. **Derive the corresponding dataset sizes.** Once you have `N_opt` for each compute bucket, compute `D_opt = C / (6N_opt)` so you can fit both model-size and token-budget scaling curves.
3. **Fit the scaling laws in log space.** A straightforward implementation is to regress `log N_opt` on `log C` and `log D_opt` on `log C`, then visualize the resulting power laws along with the observed points. The assignment explicitly asks you to extrapolate at least to `10^24` FLOPs in the warm-up exercise.
4. **Build a cache-aware live-query loop for the real assignment.** The `/loss` endpoint accepts model and optimizer settings plus a training FLOPs value, and repeated queries for an already-seen configuration do not consume extra budget. In practice, that means you should persist results locally and never spend budget on duplicate runs.
5. **Use the exploration budget strategically.** Start with coarse sweeps over model scales and a few learning-rate and batch-size choices. Once you see which regions dominate on each IsoFLOPs slice, spend the remaining budget tightening around those regions rather than exploring uniformly.
6. **Translate the predicted optimum back into concrete hyperparameters.** The final deliverable is not just “about X parameters.” You need a valid combination of `d_model`, layer count, head count, batch size, and learning rate that is consistent with the predicted optimum and with the API’s allowed ranges.
7. **Report both methodology and prediction quality.** The handout grades both the written explanation and the actual quality of the submitted prediction, so reproducibility matters as much as the final number.

## What You Learn
- Scaling laws are only useful if you can fit them from a finite, noisy, and expensive set of runs.
- Compute-optimal training is a balance between model size and number of tokens, not “make the model as large as possible.”
- A good scaling-law workflow looks like active experimental design: warm-up on synthetic data, then use live budget where it reduces uncertainty the most.
- Converting theory into a real training plan means bridging three layers at once: empirical loss curves, parameter-count formulas, and discrete architecture constraints.

## Sources
- Handout: `https://github.com/stanford-cs336/spring2024-assignment3-scaling/blob/main/cs336_spring2024_assignment3_scaling.pdf`
- Repo overview: `https://github.com/stanford-cs336/spring2024-assignment3-scaling`
- Local handout extraction: `/Users/otonashi/thinking/pratchett-os/tmp/research-009-a3/cs336_spring2024_assignment3_scaling.pdf` via `pdftotext`
- Synthetic IsoFLOPs runs: `/Users/otonashi/thinking/pratchett-os/tmp/research-009-a3/data/isoflops_curves.json`
- README: `/Users/otonashi/thinking/pratchett-os/tmp/research-009-a3/README.md`
