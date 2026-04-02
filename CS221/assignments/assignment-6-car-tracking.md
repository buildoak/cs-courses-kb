# CS221 Assignment 6: Car Tracking

## Problem Statement

This assignment is about tracking a moving car from noisy distance measurements. The setup starts with a tiny hidden-state Bayesian network, then moves to exact inference on a grid, then to particle filtering, and finally to the awkward case where multiple cars produce ambiguous readings.

The core question is simple: how do you keep a belief distribution over where the car is when your sensors are noisy and the car may move? The assignment answers it twice. Exact inference gives the mathematically clean answer. Particle filtering gives the scalable approximation.

## Key Techniques

- **Bayesian network inference.** The written problems ask for posterior probabilities over latent car position given sensor evidence.
- **Emission updates.** `observe` multiplies the belief state by the Gaussian sensor likelihood at each tile.
- **Transition updates.** `elapseTime` pushes the belief distribution through the movement model.
- **Normalization.** After each update, the posterior has to be renormalized or the beliefs stop being meaningful.
- **Particle resampling.** The particle filter keeps a discrete approximation of the posterior by reweighting and resampling particles.
- **Ambiguity-aware modeling.** The final written problem shows why multiple cars make the posterior structure much harder than the single-car case.

## Implementation Walkthrough

1. **Start with the small HMM.** The first written questions use binary car and sensor variables `C_t` and `D_t` to make the posterior algebra visible before any code.
2. **Implement `ExactInference.observe`.** The belief stored in `self.belief` is updated by multiplying each tile's probability by the sensor model, which uses a Gaussian PDF around the true distance from the agent car to the hidden car.
3. **Implement `ExactInference.elapseTime`.** The next step propagates probability mass through `self.transProb`, summing over old tiles to compute the next belief.
4. **Move to `ParticleFilter.observe` and `ParticleFilter.elapseTime`.** The approximate tracker stores particles rather than a full grid. Observations change particle weights, and resampling concentrates particles where the evidence says the car probably is.
5. **Use the visualization flags to sanity-check behavior.** The assignment is built to be watched as well as graded. `drive.py` is the fast feedback loop for checking whether the tracker is following the hidden car.
6. **Notice the scaling story.** Exact inference is fine on small maps because every tile is manageable. Particle filters are there for the cases where full-grid inference starts wasting effort on places the car almost certainly is not.

## What You Learn

- Bayesian tracking is repeated conditioning plus prediction.
- Exact inference is clean but costs proportional to the whole state space.
- Particle filters trade exactness for speed and are usually good enough for tracking.
- The moment you have multiple indistinguishable objects, the posterior stops being a neat one-object story.
- HMM intuition matters more here than fancy algebra.

## Sources

- Assignment page: `https://raw.githubusercontent.com/stanford-cs221/autumn2019/gh-pages/assignments/car/index.html`
- Course home page: `https://raw.githubusercontent.com/stanford-cs221/autumn2019/gh-pages/index.html`
- Public repo: `https://github.com/stanford-cs221/autumn2019`
