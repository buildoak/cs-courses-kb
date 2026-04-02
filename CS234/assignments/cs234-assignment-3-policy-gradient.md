# CS234 Assignment 3: Policy Gradient and Variance Reduction

## Problem Statement

This assignment switches from value-based deep RL to policy-based optimization. The core coding problem is to implement vanilla policy gradient with two variance-reduction upgrades:

1. REINFORCE with sampled returns.
2. A learned neural baseline for value prediction.
3. Advantage normalization.

The same codepath must work for both discrete and continuous control. The writeup then asks you to validate the implementation on `CartPole-v0`, `InvertedPendulum-v1`, and `HalfCheetah-v1`, and to study how batch size and baseline usage change training behavior.

## Key Techniques

- **Policy gradient theorem:** optimize expected return directly through `log pi_theta(a|s)`.
- **Monte Carlo returns:** use full-trajectory discounted returns as unbiased estimates of action value.
- **Neural policy parameterization:** categorical policy for discrete environments, diagonal Gaussian policy for continuous control.
- **Baseline subtraction:** predict state value and subtract it from returns to reduce variance.
- **Advantage normalization:** center and scale advantages to make optimization less noisy.
- **Environment transfer:** keep the same algorithm skeleton while switching from CartPole to MuJoCo-style continuous domains.

## Implementation Walkthrough

Everything important lives in `assignment3/starter_code/pg.py`:

1. **Build the policy MLP**
   - `build_mlp` stacks hidden layers with ReLU and ends with a task-specific output layer.
   - The code uses the same helper for both the policy network and the baseline network.

2. **Create placeholders**
   - Observations are always `float32`.
   - Actions are `int32` for discrete control and `float32` vectors for continuous control.
   - Advantages are one scalar per sampled timestep.

3. **Construct the policy**
   - **Discrete case:** output action logits, sample with `tf.multinomial`, and compute log-probabilities with sparse softmax cross-entropy.
   - **Continuous case:** output action means, learn log standard deviations, sample from a diagonal Gaussian, and evaluate the log-probability of the taken actions.

4. **Define the policy loss**
   - The update is implemented as minimizing `-logprob * advantage`.
   - Averaging across the batch gives the scalar objective optimized by Adam.

5. **Add the baseline network**
   - Build a second MLP from observations to a scalar prediction.
   - Train it with mean-squared error against the empirical returns.

6. **Compute returns**
   - `get_returns` walks each sampled trajectory and forms:
     - `G_t = r_t + gamma r_{t+1} + gamma^2 r_{t+2} + ...`
   - This is the Monte Carlo target used by both REINFORCE and the baseline.

7. **Compute advantages**
   - Start from returns.
   - If the baseline is enabled, subtract the predicted value.
   - If normalization is enabled, shift to zero mean and unit variance.

8. **Train in batches of trajectories**
   - `sample_path` rolls out trajectories until the configured batch size is reached.
   - `train()` concatenates observations and actions across trajectories, computes returns and advantages, optionally updates the baseline, then updates the policy.
   - The config file defaults to baseline and normalized advantages turned on.

The assignment is compact, but it teaches a deep conceptual split: DQN learns a value function and extracts a policy indirectly, while REINFORCE parameterizes the policy itself and accepts higher variance in exchange for a more direct optimization target.

## What You Learn

- How to turn the policy gradient theorem into an actual loss function.
- Why high-variance Monte Carlo gradients are painful without a baseline.
- How one codebase can support both categorical and Gaussian policies.
- Why continuous control pushes you toward policy gradients even when value-based methods dominate in Atari-style settings.
- How hyperparameters like batch size and network width change the stability of training, not just the final score.

## Sources

- GitHub search used to locate candidate repos: https://github.com/search?q=cs234+stanford&type=repositories
- Primary repo: https://github.com/arowdy98/Stanford-CS234
- Assignment handout: https://github.com/arowdy98/Stanford-CS234/blob/master/assignment3/assignment3.pdf
- Policy-gradient code: https://github.com/arowdy98/Stanford-CS234/blob/master/assignment3/starter_code/pg.py
- Assignment README: https://github.com/arowdy98/Stanford-CS234/blob/master/assignment3/starter_code/README.md
- Default config: https://github.com/arowdy98/Stanford-CS234/blob/master/assignment3/starter_code/config.py
